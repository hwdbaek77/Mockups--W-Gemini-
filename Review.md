# iTandem Technical Implementation Review

## Executive Summary

This document outlines a revised technical implementation plan for the Harvard-Westlake Carpool & Tandem Finder platform, analyzing the current specifications and proposing optimal data structures, services, and architecture patterns.

---

## I. Architecture Overview

### Recommended Stack
- **Frontend**: React/Next.js web application (progressive web app for mobile-like experience)
- **Backend**: Node.js/Express REST API
- **Database**: PostgreSQL (primary) + Redis (caching)
- **Authentication**: OAuth 2.0 with school SSO integration
- **Hosting**: AWS or Google Cloud Platform
- **Payment Processing**: Stripe Connect (escrow-based transactions)
- **Real-time Features**: WebSockets (Socket.io) for chat and notifications

### Why This Stack?
- PostgreSQL offers better ACID compliance than MongoDB for financial transactions
- Redis provides fast caching for real-time matching algorithms
- Stripe Connect handles marketplace payments with proper escrow and dispute resolution
- Progressive Web App avoids app store approval delays while providing native-like experience

---

## II. Database Schema Design

### Core Entities

#### **Users Table**
```
- user_id (UUID, primary key)
- school_email (unique, verified)
- first_name, last_name
- grade_level (sophomore/junior/senior)
- phone_number
- profile_photo_url
- bio
- music_preferences
- created_at, updated_at
- account_status (active/suspended/banned)
- stripe_account_id
```

#### **Vehicles Table**
```
- vehicle_id (UUID, primary key)
- user_id (foreign key)
- license_plate (unique, indexed)
- make, model, year, color
- vehicle_size (compact/standard/large)
- photo_url
- is_verified (boolean)
```

#### **ParkingSpots Table**
```
- spot_id (UUID, primary key)
- lot_name (Taper/Coldwater/Hacienda/StMichael/Hamilton)
- spot_number
- spot_type (single/tandem)
- coordinates (lat/lng for map rendering)
- distance_to_campus (meters)
- is_compact (boolean)
- spot_rating (calculated average)
- created_at
```

#### **SpotOwnership Table**
```
- ownership_id (UUID, primary key)
- spot_id (foreign key)
- user_id (foreign key)
- start_date, end_date
- ownership_type (permanent/rented)
- is_active (boolean)
```

#### **Schedules Table**
```
- schedule_id (UUID, primary key)
- user_id (foreign key)
- day_of_week (Monday-Friday)
- arrival_time
- departure_time
- has_lunch_off_campus (boolean)
- extracurricular_end_time
- is_manually_overridden (boolean)
```

#### **TandemPairings Table**
```
- pairing_id (UUID, primary key)
- spot_id (foreign key)
- user_1_id, user_2_id (foreign keys)
- compatibility_score (0-100)
- start_date, end_date
- status (pending/active/completed/cancelled)
- created_at
```

#### **CarpoolGroups Table**
```
- carpool_id (UUID, primary key)
- driver_user_id (foreign key)
- passenger_user_ids (array of UUIDs)
- home_location (lat/lng)
- compatibility_score (0-100)
- status (pending/active/completed/cancelled)
- created_at
```

#### **SpotRentals Table**
```
- rental_id (UUID, primary key)
- spot_id (foreign key)
- owner_user_id (foreign key)
- renter_user_id (foreign key)
- rental_date
- price_cents (integer, stored in cents)
- status (pending/confirmed/completed/cancelled/disputed)
- payment_intent_id (Stripe)
- created_at, updated_at
- cancellation_timestamp
```

#### **Transactions Table**
```
- transaction_id (UUID, primary key)
- rental_id (foreign key)
- amount_cents
- stripe_charge_id
- status (pending/completed/refunded/failed)
- created_at
```

#### **Penalties Table**
```
- penalty_id (UUID, primary key)
- user_id (foreign key)
- offense_type (late_cancellation/spot_blocking/false_report)
- amount_cents
- incident_date
- is_paid (boolean)
- created_at
```

#### **Reports Table**
```
- report_id (UUID, primary key)
- reporter_user_id (foreign key)
- reported_user_id (foreign key)
- rental_id (nullable foreign key)
- report_type (blocked_spot/damage/harassment/scam)
- description
- photo_urls (array)
- status (pending/investigating/resolved/dismissed)
- admin_notes
- created_at, resolved_at
```

#### **Messages Table**
```
- message_id (UUID, primary key)
- sender_user_id (foreign key)
- recipient_user_id (foreign key)
- conversation_type (carpool/tandem)
- message_type (text/system/emote)
- content
- is_read (boolean)
- created_at
```

#### **Notifications Table**
```
- notification_id (UUID, primary key)
- user_id (foreign key)
- notification_type (match_found/rental_request/payment/penalty/system)
- title, message
- related_entity_id (polymorphic reference)
- is_read (boolean)
- created_at
```

---

## III. Algorithm Design

### Tandem Compatibility Algorithm

**Inputs:**
- User A schedule (arrival/departure times per day)
- User B schedule
- Grade levels
- Extracurricular commitments
- Lunch preferences

**Scoring System (0-100 points):**
1. **Schedule Overlap (40 points max)**
   - Calculate time overlap when both need the spot simultaneously
   - Perfect score = 0 hours overlap per week
   - Deduct 5 points per overlapping hour

2. **Grade Level Compatibility (20 points max)**
   - Senior + Senior = 20 points
   - Junior + Junior/Sophomore = 20 points
   - Sophomore + Sophomore/Junior = 20 points
   - Other combinations = 0 points

3. **Arrival Time Compatibility (20 points max)**
   - If User A leaves before User B arrives consistently = 20 points
   - Calculate average gap between departure and arrival
   - Deduct points for negative gaps

4. **Extracurricular Alignment (10 points max)**
   - If both have similar end times = 10 points
   - Mixed schedules = 5 points

5. **Lunch Habits (10 points max)**
   - Both leave for lunch = potential conflict, 0 points
   - Only one leaves = 10 points
   - Neither leaves = 10 points

**Output:** Ranked list of compatible tandem partners with scores

### Carpool Compatibility Algorithm

**Inputs:**
- User home coordinates
- Class schedules (arrival/departure times)
- Extracurricular commitments
- Grade level
- Music/bio preferences

**Scoring System (0-100 points):**
1. **Geographic Proximity (35 points max)**
   - Calculate haversine distance between homes
   - < 1 mile = 35 points
   - 1-3 miles = 25 points
   - 3-5 miles = 15 points
   - > 5 miles = 0 points
   - Bonus: Check if homes are on same route using Google Maps Directions API

2. **Schedule Alignment (35 points max)**
   - Compare arrival times (±15 min window = full points)
   - Compare departure times
   - Check consistency across weekdays

3. **Grade Level Priority (15 points max)**
   - Senior participants get +15 bonus points
   - Helps prioritize seniors who need carpools most

4. **Personal Compatibility (15 points max)**
   - Music preference similarity (if provided)
   - Bio keyword matching
   - User-set preferences

**Output:** Ranked list of potential carpool partners

### Spot Assignment Algorithm

**Purpose:** When a rented spot is blocked, automatically reassign

**Process:**
1. Query available spots in same lot
2. Filter by distance (prefer similar distance to original)
3. Check if renter's vehicle fits (compact vs standard)
4. Select closest available spot
5. Create reassignment transaction
6. Notify both users
7. Process payment transfer

---

## IV. Service Layer Architecture

### Authentication Service
- **Responsibilities:** OAuth integration with school system (Didax), session management, API key generation
- **Technology:** Passport.js with custom OAuth strategy
- **Features:**
  - School email verification
  - Two-factor authentication (optional)
  - Permission-based access control (user/admin)

### Matching Service
- **Responsibilities:** Run compatibility algorithms for tandem and carpool matching
- **Technology:** Separate microservice with job queue (Bull/Redis)
- **Process:**
  1. User creates profile or updates schedule
  2. Job added to priority queue
  3. Algorithm runs asynchronously
  4. Results cached in Redis with TTL
  5. Real-time notification sent to user

### Payment Service
- **Responsibilities:** Handle all financial transactions, escrow management
- **Technology:** Stripe Connect with platform account
- **Flow:**
  1. Renter requests spot → Payment intent created (authorized, not captured)
  2. Spot confirmed → Capture payment, hold in escrow
  3. Rental completes successfully → Transfer to owner (minus platform fee)
  4. Dispute filed → Hold payment until resolved
  5. Cancellation → Process refund based on timing

### Notification Service
- **Responsibilities:** Push notifications, email alerts, in-app messages
- **Technology:** Firebase Cloud Messaging (FCM) for push, SendGrid for email
- **Triggers:**
  - New match found
  - Rental request received
  - Payment processed
  - Spot blocked (emergency notification)
  - Partner sending "Move your car!" emote

### Verification Service
- **Responsibilities:** Verify license plates, parking spot ownership, photo uploads
- **Technology:** Integration with school parking system API
- **Features:**
  - Cross-reference user's claimed spot with school records
  - License plate validation
  - Photo verification using AWS Rekognition (check for actual parking spots)
  - Periodic re-verification

### Dispute Resolution Service
- **Responsibilities:** Handle reports, evidence collection, automated resolutions
- **Process:**
  1. User files report with photos
  2. System attempts automated resolution (e.g., blocked spot → reassign)
  3. If cannot auto-resolve → Escalate to admin panel
  4. Admin reviews evidence and makes decision
  5. Penalties applied, payments refunded/transferred accordingly

### Analytics Service
- **Responsibilities:** Track platform metrics, spot utilization, user behavior
- **Data Collected:**
  - Rental frequency per spot
  - Average compatibility scores
  - Successful vs failed matches
  - Revenue metrics
  - User retention rates

---

## V. API Design

### RESTful Endpoints Structure

**Authentication**
- `POST /api/auth/login` - OAuth login
- `POST /api/auth/verify-email` - Email verification
- `POST /api/auth/logout`

**Users**
- `GET /api/users/me` - Current user profile
- `PUT /api/users/me` - Update profile
- `POST /api/users/me/vehicle` - Add vehicle
- `GET /api/users/:id` - Public profile view

**Schedules**
- `GET /api/schedules/me` - Get my schedule
- `PUT /api/schedules/me` - Update schedule (manual override)
- `POST /api/schedules/sync` - Sync from Didax

**Tandem**
- `GET /api/tandem/matches` - Get compatible tandem partners
- `POST /api/tandem/request` - Request tandem pairing
- `PUT /api/tandem/:pairingId/accept` - Accept request
- `DELETE /api/tandem/:pairingId` - End pairing

**Carpool**
- `GET /api/carpool/matches` - Get compatible carpool partners
- `POST /api/carpool/create` - Create carpool group
- `POST /api/carpool/:carpoolId/join` - Join carpool
- `DELETE /api/carpool/:carpoolId/leave` - Leave carpool

**Rentals**
- `GET /api/rentals/available?date=YYYY-MM-DD` - Available spots for date
- `POST /api/rentals/request` - Request to rent spot
- `GET /api/rentals/my-listings` - My spots available for rent
- `POST /api/rentals/list` - List my spot for rent
- `DELETE /api/rentals/:rentalId` - Cancel rental
- `POST /api/rentals/:rentalId/report` - Report issue

**Parking Spots**
- `GET /api/spots` - All parking spots with metadata
- `GET /api/spots/:spotId` - Specific spot details
- `GET /api/spots/map/:lotName` - Map data for lot

**Messaging**
- `GET /api/messages/conversations` - List conversations
- `GET /api/messages/:userId` - Messages with specific user
- `POST /api/messages/send` - Send message
- `POST /api/messages/emote` - Send quick emote

**Admin**
- `GET /api/admin/reports` - View pending reports
- `PUT /api/admin/reports/:reportId/resolve` - Resolve report
- `POST /api/admin/penalties/apply` - Apply penalty
- `GET /api/admin/analytics` - Platform analytics

---

## VI. Critical Technical Concerns & Solutions

### Concern 1: Blockchain Implementation (Unnecessary Complexity)
**Current Plan:** "Spot ownership and renting documented with blockchain"

**Issue:** Blockchain adds massive complexity for minimal benefit. PostgreSQL with proper ACID transactions provides sufficient audit trail and data integrity.

**Recommendation:** Use traditional database with:
- Transaction logs for full audit trail
- Immutable timestamp records
- Cryptographic signatures on critical transactions
- Regular database backups and point-in-time recovery

### Concern 2: Web3 Photos (Overengineered)
**Current Plan:** "Take photo for review using web3 photos for verification"

**Issue:** Web3 storage (IPFS) is slow, expensive, and unnecessary for this use case.

**Recommendation:** Use AWS S3 or Google Cloud Storage with:
- Client-side photo compression
- Image metadata preservation (EXIF data includes timestamp, GPS)
- SHA-256 hash stored in database for integrity verification
- Signed URLs for secure access
- Lifecycle policies for cost management

### Concern 3: License Plate Vehicle API Integration
**Current Plan:** "Fetch average gas prices from license plate using public vehicle API"

**Issue:** 
- Public vehicle APIs don't provide gas mileage data
- License plate lookups may violate privacy regulations
- Gas price calculations are imprecise and potentially misleading

**Recommendation:**
- Let users manually input vehicle make/model
- Use EPA fuel economy database for MPG estimates
- Fetch local gas prices from GasBuddy API
- Calculate estimated cost sharing transparently

### Concern 4: Automated Spot Reassignment Risks
**Current Plan:** "Automatically reassign them to an open spot, and then pay the open spot owner with the original spot payment"

**Issue:** Complex edge cases could cause financial discrepancies

**Recommendation:**
- Implement automated reassignment BUT:
  - Only reassign to spots of equal or lesser distance
  - If no suitable spot available → Full refund + platform credit
  - Log all reassignments with admin notification
  - Limit reassignments to 1 per rental (prevent cascading failures)
  - Require explicit user acceptance of reassigned spot

### Concern 5: Real-time License Plate Enforcement
**Current Plan:** "Ban their license plate until they pay"

**Issue:** System has no way to physically enforce parking without school security integration

**Recommendation:**
- Integrate with school security system if possible
- Provide daily reports to parking office with violators list
- In-app penalties (account suspension, deposit holds)
- Escalation to school administration for repeat offenders
- Use social pressure (visible violation count on profile)

---

## VII. Deployment Architecture

### Environment Strategy
- **Development:** Local Docker containers
- **Staging:** AWS ECS with CloudFront CDN
- **Production:** AWS ECS with multi-AZ deployment, auto-scaling

### CI/CD Pipeline
1. GitHub repository with branch protection
2. Automated testing on pull requests (Jest, Cypress)
3. Staging deployment on merge to `develop`
4. Manual approval for production deployment
5. Automated rollback on error rate threshold

### Monitoring & Observability
- **Application Monitoring:** Datadog or New Relic
- **Error Tracking:** Sentry
- **Log Aggregation:** CloudWatch Logs
- **Uptime Monitoring:** UptimeRobot
- **Security Scanning:** Snyk for dependency vulnerabilities

### Security Measures
- HTTPS only (SSL certificates via AWS ACM)
- API rate limiting (Express Rate Limit)
- SQL injection prevention (Parameterized queries)
- XSS protection (Content Security Policy headers)
- CSRF tokens for state-changing operations
- PII encryption at rest
- Regular security audits
- Compliance with FERPA (student data privacy)

---

## VIII. Phased Implementation Roadmap

### Phase 1: Foundation (MVP) - 2-3 months
- User authentication and profiles
- Database schema implementation
- Basic spot data management
- Admin panel for manual operations

### Phase 2: Matching Features - 2 months
- Tandem compatibility algorithm
- Carpool matching algorithm
- In-app messaging with emotes
- Match request/acceptance workflow

### Phase 3: Rental Marketplace - 2-3 months
- Stripe integration
- Rental listing and booking
- Payment processing with escrow
- Basic dispute resolution

### Phase 4: Advanced Features - 1-2 months
- Automated spot reassignment
- Photo upload and verification
- Penalty system automation
- Push notifications
- Analytics dashboard

### Phase 5: Optimization - Ongoing
- Algorithm refinement based on usage data
- Performance optimization
- User feedback implementation
- School administration feedback integration

---

## IX. Cost Estimation

### Monthly Operating Costs (Estimated)
- **Hosting (AWS):** $50-200 depending on user base
- **Database (RDS PostgreSQL):** $50-150
- **Stripe Fees:** 2.9% + $0.30 per transaction
- **Cloud Storage (S3):** $10-30
- **SendGrid (Email):** $15-50
- **Firebase (Push notifications):** $0-25
- **Domain & SSL:** $15/year
- **Monitoring Tools:** $0-50 (free tiers available)

**Total:** ~$200-500/month for first year

### Revenue Model Options
1. **Transaction Fee:** 5-10% of rental marketplace transactions
2. **Premium Features:** $2.99/month for priority matching
3. **School Partnership:** Annual license fee paid by school
4. **Freemium:** Free matching, paid rental marketplace access

---

## X. Testing Strategy

### Unit Tests
- Algorithm correctness (compatibility scoring)
- Payment calculation logic
- Date/time handling for schedules

### Integration Tests
- API endpoint responses
- Database transactions
- Stripe payment flows
- OAuth authentication

### End-to-End Tests
- Complete user journeys (signup → match → rental)
- Payment flows from request to completion
- Dispute resolution workflows

### User Acceptance Testing
- Beta test with 20-30 students
- Gather feedback on matching quality
- Test edge cases in real-world scenarios
- Validate school system integration

---

## XI. Legal & Compliance Checklist

- [ ] Terms of Service drafted and reviewed
- [ ] Privacy Policy (FERPA compliant)
- [ ] Liability waiver for carpool participants
- [ ] School administration approval obtained
- [ ] Parking office coordination established
- [ ] Payment processing agreement (Stripe terms)
- [ ] User age verification (handle minors)
- [ ] Dispute resolution process documented
- [ ] Data retention and deletion policies
- [ ] Insurance considerations addressed

---

## XII. Success Metrics

### Key Performance Indicators
- **User Adoption:** 30% of parking permit holders in first semester
- **Match Success Rate:** 70%+ of matches result in active partnerships
- **Rental Utilization:** 40% of spots rented at least once per month
- **User Satisfaction:** 4+ stars average rating
- **Dispute Rate:** <5% of transactions result in disputes
- **Revenue:** Break-even within first year

### Data Collection Points
- User registration and activation rates
- Match acceptance vs rejection rates
- Average compatibility scores of successful matches
- Rental frequency and pricing trends
- Time-to-match for users seeking partners
- Support ticket volume and resolution time

---

## XIII. Risk Mitigation

### Technical Risks
| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| School API unreliable | Medium | High | Implement caching, manual schedule entry fallback |
| Payment fraud | Low | High | Stripe Radar fraud detection, hold period for new users |
| Database failure | Low | Critical | Multi-AZ deployment, automated backups, failover |
| Algorithm bias | Medium | Medium | Regular audits, user feedback, manual override option |
| Security breach | Low | Critical | Security audits, penetration testing, bug bounty |

### Business Risks
| Risk | Probability | Impact | Mitigation |
|------|-------------|--------|------------|
| School prohibits platform | Medium | Critical | Early engagement with administration, pilot program |
| Low user adoption | Medium | High | Marketing campaign, incentives, student ambassadors |
| Legal challenges | Low | High | Legal review before launch, liability insurance |
| Competitor emerges | Low | Medium | First-mover advantage, tight school integration |

---

## XIV. Recommendations Summary

### High Priority Changes
1. **Replace MongoDB with PostgreSQL** for financial transaction integrity
2. **Remove blockchain/web3 implementations** - unnecessary complexity
3. **Implement proper escrow system** using Stripe Connect
4. **Add comprehensive dispute resolution workflow**
5. **Integrate with school security** for enforcement

### Architecture Improvements
1. **Separate matching service** from main API for scalability
2. **Implement caching layer** (Redis) for performance
3. **Use message queues** for async operations
4. **Add proper monitoring** from day one

### Feature Prioritization
1. **Phase 1:** Authentication, profiles, basic matching
2. **Phase 2:** Tandem/carpool matching with chat
3. **Phase 3:** Rental marketplace (most complex)
4. **Phase 4:** Advanced features and optimization

### Critical Success Factors
- School administration buy-in and partnership
- Seamless Didax integration for minimal friction
- High-quality matching algorithms that actually work
- Robust dispute resolution to build trust
- Clear, transparent financial transactions

---

## Conclusion

This platform has strong potential but requires careful execution. The matching features (tandem/carpool) are lower-risk and should be prioritized. The rental marketplace is high-complexity and should only be implemented after the matching features prove successful. Focus on solving real problems simply rather than over-engineering with blockchain and web3 solutions that add complexity without proportional value.

**Next Steps:**
1. Validate school administration support
2. Conduct user research with 20-30 target users
3. Build minimal viable matching system
4. Test with beta users before adding rental features
5. Iterate based on real-world feedback
