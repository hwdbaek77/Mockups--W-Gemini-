{\rtf1\ansi\ansicpg1252\cocoartf2867
\cocoatextscaling0\cocoaplatform0{\fonttbl\f0\fswiss\fcharset0 Helvetica-Bold;\f1\fswiss\fcharset0 Helvetica;\f2\froman\fcharset0 Times-Roman;
}
{\colortbl;\red255\green255\blue255;\red0\green0\blue0;}
{\*\expandedcolortbl;;\cssrgb\c0\c0\c0;}
{\*\listtable{\list\listtemplateid1\listhybrid{\listlevel\levelnfc23\levelnfcn23\leveljc0\leveljcn0\levelfollow0\levelstartat0\levelspace360\levelindent0{\*\levelmarker \{disc\}}{\leveltext\leveltemplateid1\'01\uc0\u8226 ;}{\levelnumbers;}\fi-360\li720\lin720 }{\listlevel\levelnfc23\levelnfcn23\leveljc0\leveljcn0\levelfollow0\levelstartat0\levelspace360\levelindent0{\*\levelmarker \{circle\}}{\leveltext\leveltemplateid2\'01\uc0\u9702 ;}{\levelnumbers;}\fi-360\li1440\lin1440 }{\listlevel\levelnfc23\levelnfcn23\leveljc0\leveljcn0\levelfollow0\levelstartat0\levelspace360\levelindent0{\*\levelmarker \{square\}}{\leveltext\leveltemplateid3\'01\uc0\u9642 ;}{\levelnumbers;}\fi-360\li2160\lin2160 }{\listname ;}\listid1}
{\list\listtemplateid2\listhybrid{\listlevel\levelnfc23\levelnfcn23\leveljc0\leveljcn0\levelfollow0\levelstartat0\levelspace360\levelindent0{\*\levelmarker \{disc\}}{\leveltext\leveltemplateid101\'01\uc0\u8226 ;}{\levelnumbers;}\fi-360\li720\lin720 }{\listlevel\levelnfc23\levelnfcn23\leveljc0\leveljcn0\levelfollow0\levelstartat0\levelspace360\levelindent0{\*\levelmarker \{circle\}}{\leveltext\leveltemplateid102\'01\uc0\u9702 ;}{\levelnumbers;}\fi-360\li1440\lin1440 }{\listname ;}\listid2}
{\list\listtemplateid3\listhybrid{\listlevel\levelnfc23\levelnfcn23\leveljc0\leveljcn0\levelfollow0\levelstartat0\levelspace360\levelindent0{\*\levelmarker \{disc\}}{\leveltext\leveltemplateid201\'01\uc0\u8226 ;}{\levelnumbers;}\fi-360\li720\lin720 }{\listlevel\levelnfc23\levelnfcn23\leveljc0\leveljcn0\levelfollow0\levelstartat0\levelspace360\levelindent0{\*\levelmarker \{circle\}}{\leveltext\leveltemplateid202\'01\uc0\u9702 ;}{\levelnumbers;}\fi-360\li1440\lin1440 }{\listname ;}\listid3}}
{\*\listoverridetable{\listoverride\listid1\listoverridecount0\ls1}{\listoverride\listid2\listoverridecount0\ls2}{\listoverride\listid3\listoverridecount0\ls3}}
\margl1440\margr1440\vieww11520\viewh8400\viewkind0
\deftab720
\pard\pardeftab720\partightenfactor0

\f0\b\fs29\fsmilli14667 \cf0 \expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 API & Backend - Max
\f1\b0 \uc0\u8232 \u8232 Firestore Database with MongoDB and schema for storing each parking spot and info about it
\f2\fs24 \
\pard\pardeftab720\partightenfactor0

\f1\fs29\fsmilli14667 \cf0 Schema for each user and personal data about them
\f2\fs24 \

\f1\fs29\fsmilli14667 Spots can be temporarily linked to a user with a permanent history of who has had possession of a spot
\f2\fs24 \
\

\f1\fs29\fsmilli14667 RESTful API for communicating with client apps to serve data
\f2\fs24 \

\f1\fs29\fsmilli14667 All computations and fetching data from Didax school api are done automatically on a backend server
\f2\fs24 \

\f1\fs29\fsmilli14667 Backend server hosted using firebase
\f2\fs24 \

\f1\fs29\fsmilli14667 Each user has an API key with their own permissions
\f2\fs24 \

\f1\fs29\fsmilli14667 Admin panel hosted on server for management as a web app
\f2\fs24 \

\f1\fs29\fsmilli14667 Scheduler on server for managing computations (such as tandem and carpool compatibility calculations, requests to rent spots, etc with priority using a priority queue)
\f2\fs24 \

\f1\fs29\fsmilli14667 Manage billing with stripe
\f2\fs24 \

\f1\fs29\fsmilli14667 Server backend logic uses SQL data about spots and users upon request, to calculate compatibility for carpool/tandem, renting, calling external apis such as didax, authentication, etc.
\f2\fs24 \
\
\pard\pardeftab720\partightenfactor0

\f0\b\fs29\fsmilli14667 \cf0 Design & UI - Lauren
\f2\b0\fs24 \
\

\f0\b\fs29\fsmilli14667 Tandem - Nathan
\f2\b0\fs24 \
\pard\tx220\tx720\pardeftab720\li720\fi-720\partightenfactor0
\ls1\ilvl0
\f1\fs29\fsmilli14667 \cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u8226 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Chat emotes\
\pard\tx940\tx1440\pardeftab720\li1440\fi-1440\partightenfactor0
\ls1\ilvl1\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 No custom chats\
\ls1\ilvl1\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 An algorithm to track spamming\
\ls1\ilvl1\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Move your car!\
\ls1\ilvl1\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Leaving soon!\
\pard\tx220\tx720\pardeftab720\li720\fi-720\partightenfactor0
\ls1\ilvl0\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u8226 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 What tandem partners are most compatible with each other\
\pard\tx940\tx1440\pardeftab720\li1440\fi-1440\partightenfactor0
\ls1\ilvl1\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 An algorithm to compare compatibility by schedule\
\pard\tx1660\tx2160\pardeftab720\li2160\fi-2160\partightenfactor0
\ls1\ilvl2\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9642 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 When they come and when they leave\
\ls1\ilvl2\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9642 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 extracurricular activities\
\ls1\ilvl2\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9642 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Going off for lunch\
\ls1\ilvl2\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9642 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Seniors should be paired with other seniors\
\ls1\ilvl2\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9642 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Juniors can be paired with juniors + sophomores\
\ls1\ilvl2\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9642 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Sophomores can be paired with sophomores + juniors\
\ls1\ilvl2\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9642 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Arrival time\
\pard\tx220\tx720\pardeftab720\li720\fi-720\partightenfactor0
\ls1\ilvl0\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u8226 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 \
\pard\pardeftab720\partightenfactor0

\f2\fs24 \cf0 \
\pard\pardeftab720\partightenfactor0

\f0\b\fs29\fsmilli14667 \cf0 Carpool - Daniel
\f2\b0\fs24 \
\pard\tx220\tx720\pardeftab720\li720\fi-720\partightenfactor0
\ls2\ilvl0
\f1\fs29\fsmilli14667 \cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u8226 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Factors for choosing carpools (in order by weight)\
\pard\tx940\tx1440\pardeftab720\li1440\fi-1440\partightenfactor0
\ls2\ilvl1\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Location\
\ls2\ilvl1\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Class schedule\
\ls2\ilvl1\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Extracurricular commitments\
\ls2\ilvl1\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Give priority to seniors w/ carpools or just seniors in general
\f0\b \
\pard\tx220\tx720\pardeftab720\li720\fi-720\partightenfactor0
\ls2\ilvl0
\f1\b0 \cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u8226 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Miscellaneous factors\
\pard\tx940\tx1440\pardeftab720\li1440\fi-1440\partightenfactor0
\ls2\ilvl1\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Fetch average gas prices in the area from license plate using public vehicle api and then get combined city/highway to calculate approximate gas price\
\ls2\ilvl1\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Chat for communicating with partner\
\pard\tx220\tx720\pardeftab720\li720\fi-720\partightenfactor0
\ls2\ilvl0\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u8226 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Have a bio section for music, etc.\
\pard\pardeftab720\partightenfactor0

\f2\fs24 \cf0 \
\pard\pardeftab720\partightenfactor0

\f0\b\fs29\fsmilli14667 \cf0 Parking Spot Rental - Daniel
\f2\b0\fs24 \
\pard\tx220\tx720\pardeftab720\li720\fi-720\partightenfactor0
\ls3\ilvl0
\f1\fs29\fsmilli14667 \cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u8226 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Availability\'a0\
\pard\tx940\tx1440\pardeftab720\li1440\fi-1440\partightenfactor0
\ls3\ilvl1\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Cancellation day before: full refund\
\ls3\ilvl1\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Cancellation day of: renter gets fine\
\ls3\ilvl1\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Spot ownership and renting documented with blockchain\
\pard\tx220\tx720\pardeftab720\li720\fi-720\partightenfactor0
\ls3\ilvl0\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u8226 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Report system\
\pard\tx940\tx1440\pardeftab720\li1440\fi-1440\partightenfactor0
\ls3\ilvl1\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 If a student finds a spot they rented blocked, automatically reassign them to an open spot, and then pay the open spot owner with the original spot payment\
\ls3\ilvl1\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 If a student other than the original renter blocks the spot, fine that student directly and ban their license plate until they pay\
\ls3\ilvl1\kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Take photo for review using web3 photos for verification\
\pard\tx220\tx720\pardeftab720\li720\fi-720\partightenfactor0
\ls3\ilvl0\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u8226 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Renting system\
\pard\tx940\tx1440\pardeftab720\li1440\fi-1440\partightenfactor0
\ls3\ilvl1\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Spots that are available are highlighted in green, which they can do and they get a confirmation\
\pard\tx220\tx720\pardeftab720\li720\fi-720\partightenfactor0
\ls3\ilvl0\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u8226 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Spots get more expensive by distance\
\pard\tx940\tx1440\pardeftab720\li1440\fi-1440\partightenfactor0
\ls3\ilvl1\cf0 \kerning1\expnd0\expndtw0 \outl0\strokewidth0 {\listtext	\uc0\u9702 	}\expnd0\expndtw0\kerning0
\outl0\strokewidth0 \strokec2 Market rate can be fine-tuned\
}