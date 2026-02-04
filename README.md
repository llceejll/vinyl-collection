 <!DOCTYPE html>

<html lang="en">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Vinyl Collection | 311 Albums with Artwork</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <script src="https://unpkg.com/react@18/umd/react.production.min.js"></script>
  <script src="https://unpkg.com/react-dom@18/umd/react-dom.production.min.js"></script>
  <script src="https://unpkg.com/@babel/standalone/babel.min.js"></script>
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&display=swap" rel="stylesheet">
  <style>
    * { font-family: 'Inter', sans-serif; }
    @keyframes fadeIn { from { opacity: 0; transform: scale(0.95); } to { opacity: 1; transform: scale(1); } }
    @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    .animate-fadeIn { animation: fadeIn 0.3s ease-out forwards; }
    .animate-slideUp { animation: slideUp 0.4s ease-out forwards; }
    .album-card:hover .vinyl-disc { transform: translateX(30%); }
    .album-card:hover .album-art { transform: scale(1.05); }
  </style>
</head>
<body class="bg-black">
  <div id="root"></div>
  <script type="text/babel">
    const { useState, useMemo } = React;

```
// Pre-fetched artwork URLs from iTunes
```

const artworkUrls = {
“AC/DC-Back in Black”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/1e/14/58/1e145814-281a-58e0-3ab1-145f5d1af421/886443673441.jpg/600x600bb.jpg”,
“AC/DC-Highway to Hell”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/b9/c8/ef/b9c8ef42-bbc9-64df-11f8-717571f6730f/886443673458.jpg/600x600bb.jpg”,
“AC/DC-For Those About to Rock”: “https://is1-ssl.mzstatic.com/image/thumb/Features125/v4/14/7e/d7/147ed798-ff65-ad74-6fd3-86e452dca077/dj.hnuoonld.jpg/600x600bb.jpg”,
“AC/DC-Dirty Deeds Done Dirt Cheap”: “https://is1-ssl.mzstatic.com/image/thumb/Music/v4/d8/46/3a/d8463adf-5f24-6a42-6718-4ce33fe34ff7/Cover.jpg/600x600bb.jpg”,
“Led Zeppelin-Physical Graffiti”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/82/6c/5d/826c5d66-67b7-62f2-4d78-dd3c7b415e08/dj.pifhrvpa.jpg/600x600bb.jpg”,
“Led Zeppelin-Led Zeppelin IV”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/5c/15/9b/5c159b27-95ca-b9a7-84e3-28e795fffd39/dj.kvkrpptq.jpg/600x600bb.jpg”,
“Led Zeppelin-In Through the Out Door”: “https://is1-ssl.mzstatic.com/image/thumb/Music7/v4/32/33/78/32337882-4dcf-b4ff-c2b1-988297b1aa2a/dj.bazmvvpg.jpg/600x600bb.jpg”,
“Van Halen-Van Halen”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/7a/ef/88/7aef88ad-25aa-be91-eb78-8917c3f114f7/603497894130.jpg/600x600bb.jpg”,
“Def Leppard-Pyromania”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/99/ef/71/99ef71ff-eaea-7bb7-c479-c079f14b0a5b/17UM1IM41551.rgb.jpg/600x600bb.jpg”,
“Iron Maiden-The Number of the Beast”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/12/6b/44/126b4441-6747-c411-b765-7e54aefbf79f/881034134448.jpg/600x600bb.jpg”,
“Ozzy Osbourne-Diary of a Madman”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/68/5d/b6/685db61f-ea79-a16a-f1c4-a86cab8e0311/886449629367.jpg/600x600bb.jpg”,
“Scorpions-Blackout”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/8d/a4/f9/8da4f95f-4162-9d31-7131-fe49337a1689/00731453478626.rgb.jpg/600x600bb.jpg”,
“Meat Loaf-Bat Out of Hell”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/cd/90/b5/cd90b5c4-9379-92ae-da01-4b27ea00697b/886443775787.jpg/600x600bb.jpg”,
“Aerosmith-Night in the Ruts”: “https://is1-ssl.mzstatic.com/image/thumb/Music112/v4/60/bf/05/60bf0566-8cdd-0292-48f7-f2d853444e92/22UM1IM35692.rgb.jpg/600x600bb.jpg”,
“Alice Cooper-School’s Out”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/27/5c/be/275cbe43-0f2e-c721-b086-831555964a0a/603497830633.jpg/600x600bb.jpg”,
“Alice Cooper-Billion Dollar Babies”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/f2/b4/94/f2b49450-1cba-b3d1-bc68-0ae46f8873ab/603497825257.jpg/600x600bb.jpg”,
“Alice Cooper-Welcome to My Nightmare”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/c7/b9/d1/c7b9d142-85db-6bae-97fa-471c5c8da2c6/075678154263.jpg/600x600bb.jpg”,
“Alice Cooper-Goes to Hell”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/da/18/ba/da18baf9-e8ac-1611-d297-74de6789b334/603497073269.jpg/600x600bb.jpg”,
“Bad Company-Bad Company”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/cf/85/02/cf8502d1-c7e1-1733-3d64-7a9c6b3a1b21/603497892419.jpg/600x600bb.jpg”,
“Bad Company-Straight Shooter”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/6a/cd/da/6acddac4-3d77-93bb-2880-4659c392bcd6/603497892402.jpg/600x600bb.jpg”,
“Grand Funk Railroad-All the Girls in the World Beware”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/dc/75/97/dc759759-4938-5916-39ba-2105a6bde98f/00724358053258.jpg/600x600bb.jpg”,
“Grand Funk Railroad-Caught in the Act”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/a1/14/b9/a114b9ef-1eac-28d2-1be3-d166a99d4d0e/00724358059250.rgb.jpg/600x600bb.jpg”,
“Joan Jett and the Blackhearts-I Love Rock n Roll”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/09/8b/86/098b862d-7e7a-ff59-24cb-87eb83c7ee58/886447254332.jpg/600x600bb.jpg”,
“Aldo Nova-Aldo Nova”: “https://is1-ssl.mzstatic.com/image/thumb/Music/a8/ed/d2/mzi.rbyupxch.jpg/600x600bb.jpg”,
“Billy Squier-Don’t Say No”: “https://is1-ssl.mzstatic.com/image/thumb/Music112/v4/bc/80/46/bc8046a2-3994-6566-c290-6f8fa6a23324/14UMGIM11047.rgb.jpg/600x600bb.jpg”,
“Billy Squier-Emotions in Motion”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/24/78/d1/2478d192-1e30-bf24-17f4-87817f0d232f/00602537814404.rgb.jpg/600x600bb.jpg”,
“Night Ranger-Dawn Patrol”: “https://is1-ssl.mzstatic.com/image/thumb/Music118/v4/0c/dc/56/0cdc5645-bb8a-604b-5db3-d7cb8f459ab7/00076731103129.rgb.jpg/600x600bb.jpg”,
“New England-New England”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/88/a9/76/88a976ec-bfd6-320d-70fa-e6983e9a8a07/00602557368659.rgb.jpg/600x600bb.jpg”,
“Kiss-Destroyer”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/7b/a5/9b/7ba59b98-a9ae-04d0-d962-a304785830d7/21UM1IM23268.rgb.jpg/600x600bb.jpg”,
“Kiss-Rock and Roll Over”: “https://is1-ssl.mzstatic.com/image/thumb/Music118/v4/44/3e/45/443e45d2-11e2-0db6-f398-b83746aca676/00602537855667.rgb.jpg/600x600bb.jpg”,
“Kiss-Love Gun”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/4f/4e/09/4f4e09f3-43eb-ea9a-8e4a-dbd4553d2b3d/00602547058072.rgb.jpg/600x600bb.jpg”,
“Kiss-Alive!”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/ea/70/91/ea70918a-2d22-7bab-35ec-24c890880d12/06UMGIM02296.rgb.jpg/600x600bb.jpg”,
“Kiss-Alive II”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/c9/75/08/c97508fd-1823-b298-3415-1be19931e7cf/00602537855698.rgb.jpg/600x600bb.jpg”,
“Kiss-Dynasty”: “https://is1-ssl.mzstatic.com/image/thumb/Music122/v4/92/7f/5c/927f5c6f-e24a-8db5-e165-38d8b9314d8d/14UMGIM16041.rgb.jpg/600x600bb.jpg”,
“Kiss-The Originals”: “https://is1-ssl.mzstatic.com/image/thumb/Music123/v4/d7/b8/98/d7b898f0-0f5e-f8a8-cf30-871c5c5bf12d/603497851775.jpg/600x600bb.jpg”,
“Ace Frehley-Ace Frehley”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/65/af/a2/65afa265-d0dd-4707-626a-9dfbf8d558a6/00602537855742.rgb.jpg/600x600bb.jpg”,
“Gene Simmons-Gene Simmons”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/a3/30/54/a33054b1-d501-b7ea-5eb3-3ed36201c4ba/06UMGIM15802.rgb.jpg/600x600bb.jpg”,
“Paul Stanley-Paul Stanley”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/a3/30/54/a33054b1-d501-b7ea-5eb3-3ed36201c4ba/06UMGIM15802.rgb.jpg/600x600bb.jpg”,
“Peter Criss-Peter Criss”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/6d/ea/5e/6dea5e7a-12c2-2be9-f681-02ddd55f379f/artwork.jpg/600x600bb.jpg”,
“Pink Floyd-The Dark Side of the Moon”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/0b/5b/66/0b5b6697-9d7e-86aa-1971-7a370d6b1d34/859744553750_cover.jpg/600x600bb.jpg”,
“Pink Floyd-The Wall”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/3e/17/ec/3e17ec6d-f980-c64f-19e0-a6fd8bbf0c10/886445635850.jpg/600x600bb.jpg”,
“Rush-Moving Pictures”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/9e/84/e9/9e84e984-e923-de7d-16e5-982b2cdd1d98/21UMGIM68387.rgb.jpg/600x600bb.jpg”,
“Rush-Hemispheres”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/17/98/25/1798259b-6cd8-d7c0-b211-88f49eea18a0/12UMGIM19109.rgb.jpg/600x600bb.jpg”,
“Yes-90125”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/9e/82/79/9e82797b-ff48-eb1a-4e17-59dbf62d72df/603497886081.jpg/600x600bb.jpg”,
“Genesis-Abacab”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/d5/ed/ab/d5edab8a-e47e-cb87-66c2-3bac86e1b5e2/081227998684.jpg/600x600bb.jpg”,
“Genesis-Genesis”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/8b/7b/ce/8b7bcee5-4c31-aab1-7a65-82e757057e83/mzi.ykiyfzna.jpg/600x600bb.jpg”,
“Supertramp-Breakfast in America”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/10/bd/ee/10bdee2b-e13b-b1c2-b264-484e37f5208e/10UMGIM21523.rgb.jpg/600x600bb.jpg”,
“Supertramp-Crime of the Century”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/39/cd/49/39cd4988-2b67-33bf-1aaa-c401b397c35d/00600753547687.rgb.jpg/600x600bb.jpg”,
“Supertramp-Even in the Quietest Moments”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/2b/64/da/2b64da94-bc08-19f9-542e-355e2a616b7f/06UMGIM15879.rgb.jpg/600x600bb.jpg”,
“Styx-Paradise Theatre”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/7b/2c/ba/7b2cba1f-d3f8-a043-b71d-d596f22e6142/06UMGIM18641.rgb.jpg/600x600bb.jpg”,
“Styx-The Grand Illusion”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/8c/32/78/8c327836-f69c-a8aa-79f1-2fa65f1fa9ab/06UMGIM01288.rgb.jpg/600x600bb.jpg”,
“Styx-Pieces of Eight”: “https://is1-ssl.mzstatic.com/image/thumb/Music122/v4/9a/be/99/9abe9905-7c26-8204-1133-364e200793d0/06UMGIM01287.rgb.jpg/600x600bb.jpg”,
“Styx-Cornerstone”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/69/4d/52/694d5298-12a3-0070-e051-352496c0b406/00075021323926.rgb.jpg/600x600bb.jpg”,
“Styx-Crystal Ball”: “https://is1-ssl.mzstatic.com/image/thumb/Music118/v4/b5/14/4b/b5144bc0-05d3-1d9c-1e08-18f7115023f3/00075021321823.rgb.jpg/600x600bb.jpg”,
“Styx-Equinox”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/ef/49/42/ef494230-0725-a760-4d97-06a52b177a45/00075021321724.rgb.jpg/600x600bb.jpg”,
“The Alan Parsons Project-Eye in the Sky”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/fe/c4/46/fec446f8-30e6-b526-0f71-58e7ff0b17f5/886446379685.jpg/600x600bb.jpg”,
“The Alan Parsons Project-I Robot”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/dc/c5/af/dcc5afe5-cc68-b4ef-aae4-1f98593cb8f1/886446301341.jpg/600x600bb.jpg”,
“The Moody Blues-Long Distance Voyager”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/a0/b3/22/a0b3228c-1806-013b-7fa7-c4afa31c54fb/00602567756699.rgb.jpg/600x600bb.jpg”,
“Electric Light Orchestra-Out of the Blue”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/2c/0f/f9/2c0ff9b3-693b-5690-a17d-641076ae3140/886445593945.jpg/600x600bb.jpg”,
“Electric Light Orchestra-ELO’s Greatest Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/4d/45/41/4d45414c-6364-51cf-2874-7c8aa755caaa/mzi.oetoyyot.jpg/600x600bb.jpg”,
“Electric Light Orchestra-Ole ELO”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/a3/6e/74/a36e7450-3818-f2c1-a137-d4ef5934c119/827969448922.jpg/600x600bb.jpg”,
“The Police-Synchronicity”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/a4/67/ba/a467ba62-87df-9d10-98d2-c517f68ac870/16UMGIM60882.rgb.jpg/600x600bb.jpg”,
“The Police-Ghost in the Machine”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/32/dc/c1/32dcc188-0ad1-7d9c-e3b4-a7189d815892/742779536501.png/600x600bb.jpg”,
“The Police-Zenyatta Mondatta”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/2b/6f/c1/2b6fc116-cb69-91eb-5401-e71383a7d1bc/16UMGIM60883.rgb.jpg/600x600bb.jpg”,
“Duran Duran-Rio”: “https://is1-ssl.mzstatic.com/image/thumb/Features125/v4/c5/cf/12/c5cf12af-aa53-9324-1bbe-6886c0ab8168/dj.yciqxcrk.jpg/600x600bb.jpg”,
“Duran Duran-Seven and the Ragged Tiger”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/89/5c/05/895c054a-86c6-235c-629e-f6c2af34b6b6/077774601559.jpg/600x600bb.jpg”,
“Duran Duran-Duran Duran”: “https://is1-ssl.mzstatic.com/image/thumb/Music4/v4/1b/36/92/1b36928f-b56c-11cd-b7d7-f2a4bf470a73/077774604253.jpg/600x600bb.jpg”,
“Eurythmics-Sweet Dreams Are Made of This”: “https://is1-ssl.mzstatic.com/image/thumb/Features125/v4/ad/d3/3d/add33dea-0a4d-9509-643b-939ba6735733/dj.vpugapfp.jpg/600x600bb.jpg”,
“Eurythmics-Touch”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/3e/8f/bd/3e8fbd62-ca45-c5d8-3042-ee92d6c2d30e/886447028049.jpg/600x600bb.jpg”,
“Tears for Fears-Songs from the Big Chair”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/74/41/43/744143ae-afad-8380-106e-1c1bc48922e4/14UMGIM34762.rgb.jpg/600x600bb.jpg”,
“Tears for Fears-The Hurting”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/68/3a/df/683adff9-ce14-9ce1-bba3-1d5d8a681300/23UMGIM25488.rgb.jpg/600x600bb.jpg”,
“The Cars-Heartbeat City”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/4a/56/52/4a56520d-7c2b-ae69-0057-4c9827e4ad06/603497879038.jpg/600x600bb.jpg”,
“The Cars-The Cars”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/6b/3c/1d/6b3c1d3e-46a3-d0ac-e386-c2e60c8c8f49/mzm.hdqcumzx.jpg/600x600bb.jpg”,
“The Cars-Candy-O”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/e0/25/6a/e0256a45-7361-261c-6af2-ca712d68ae1b/603497879090.jpg/600x600bb.jpg”,
“The Cars-Shake It Up”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/db/af/1e/dbaf1eb7-c181-00c6-448a-818d0f75491f/603497860210.jpg/600x600bb.jpg”,
“Culture Club-Colour by Numbers”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/ea/dc/39/eadc39bf-f349-a141-a5c4-12b3d9b018cb/14ULAIM00351.rgb.jpg/600x600bb.jpg”,
“Culture Club-Kissing to Be Clever”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/1e/d8/ff/1ed8ffe4-49ed-812e-e426-ca8e92031387/13ULAIM50593.rgb.jpg/600x600bb.jpg”,
“Culture Club-Waking Up with the House on Fire”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/27/32/11/2732119a-efd7-fc4c-9475-750bb97ebcbe/13UABIM54753.rgb.jpg/600x600bb.jpg”,
“Billy Idol-Rebel Yell”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/9c/bb/c8/9cbbc833-c06c-9362-ba76-b6f20d119343/24UMGIM00501.rgb.jpg/600x600bb.jpg”,
“Billy Idol-Billy Idol”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/cb/67/fd/cb67fd0e-cdbb-60df-c3f4-46ef0f7a14d8/13UACIM03592.rgb.jpg/600x600bb.jpg”,
“A Flock of Seagulls-Listen”: “https://is1-ssl.mzstatic.com/image/thumb/Music/f6/dd/5a/mzi.iazbbtcx.jpg/600x600bb.jpg”,
“Thompson Twins-Into the Gap”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/41/6f/0d/416f0d35-d073-8ad9-ed7f-96c170ee6491/4099964078237.jpg/600x600bb.jpg”,
“Spandau Ballet-True”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/a0/d0/03/a0d003fa-430f-bf0b-b150-94ff9a66e14e/dj.bmegvknk.jpg/600x600bb.jpg”,
“INXS-Kick”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/04/ca/65/04ca65d0-c796-3f9b-0446-f013dfc1a8d9/603497838899.jpg/600x600bb.jpg”,
“Blondie-The Best of Blondie”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/86/21/c4/8621c47c-19a2-ebca-a63a-45ae7017556f/14ULAIM00403.rgb.jpg/600x600bb.jpg”,
“The Fixx-Reach the Beach”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/e2/02/0d/e2020da1-34ec-c1b9-ddd9-76aaa52904a7/00008811236823.rgb.jpg/600x600bb.jpg”,
“Naked Eyes-Naked Eyes”: “https://is1-ssl.mzstatic.com/image/thumb/Music112/v4/4a/8a/3c/4a8a3c90-e939-0f66-34ce-58ead22e914e/5060516092673.png/600x600bb.jpg”,
“Wang Chung-Points on the Curve”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/39/8f/7a/398f7a40-d9e3-d2bd-38af-7e901b418a6f/20UMGIM75153.rgb.jpg/600x600bb.jpg”,
“Simple Minds-New Gold Dream”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/fd/bb/72/fdbb7297-8ecb-880d-2595-3512bd0cb75e/00602547665942.rgb.jpg/600x600bb.jpg”,
“Simple Minds-Sparkle in the Rain”: “https://is1-ssl.mzstatic.com/image/thumb/Music118/v4/67/3f/30/673f3032-dbaa-c9de-6f64-785d936c0163/00602547112101.rgb.jpg/600x600bb.jpg”,
“The B-52’s-The B-52’s”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/75/88/d8/7588d898-6ed3-cfef-f3d3-9938a63902d1/603497890422.jpg/600x600bb.jpg”,
“The Boomtown Rats-The Fine Art of Surfacing”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/f4/96/c7/f496c7bf-ac53-5a75-5235-c3d23ad94b25/00602498267752.rgb.jpg/600x600bb.jpg”,
“The The-Soul Mining”: “https://is1-ssl.mzstatic.com/image/thumb/Music122/v4/d7/74/12/d774120d-7df8-90ec-15fe-6c0323799234/074643926621.jpg/600x600bb.jpg”,
“The Monroes-The Monroes”: “https://is1-ssl.mzstatic.com/image/thumb/Music118/v4/8f/06/7a/8f067a13-3af5-84ed-1647-8754a19bcada/5054960473669_cover.jpg/600x600bb.jpg”,
“Big Country-The Crossing”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/55/19/bd/5519bd0b-d499-b946-400e-453c7a1e20b9/00602527890876.rgb.jpg/600x600bb.jpg”,
“Big Country-Steeltown”: “https://is1-ssl.mzstatic.com/image/thumb/Music118/v4/94/99/f8/9499f82c-d564-ac50-9cd3-d67ffd50352c/00600753506608.rgb.jpg/600x600bb.jpg”,
“The Alarm-Declaration”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/5f/98/eb/5f98ebd0-8996-4092-cc36-bbc655e206f5/192641021398_Cover.jpg/600x600bb.jpg”,
“Cyndi Lauper-She’s So Unusual”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/06/aa/19/06aa194a-56af-5dfd-aeae-7fae8c8ea97a/886444329651.jpg/600x600bb.jpg”,
“Elton John-Goodbye Yellow Brick Road”: “https://is1-ssl.mzstatic.com/image/thumb/Music122/v4/b3/60/b6/b360b655-4d28-179d-d0d4-4e128ebdf4c9/13UAEIM19273.rgb.jpg/600x600bb.jpg”,
“Elton John-Captain Fantastic and the Brown Dirt Cowboy”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/7a/cf/dd/7acfdd96-a5c7-c79f-4d07-96e49601ac5d/06UMGIM53454.rgb.jpg/600x600bb.jpg”,
“Elton John-Empty Sky”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/62/83/f7/6283f7ee-1e61-297c-e64e-da5680d248e6/06UMGIM65008.rgb.jpg/600x600bb.jpg”,
“Elton John-Elton John”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/a9/52/e6/a952e6c5-a5e1-a56d-a6f4-b01ca42a4368/06UMGIM50695.rgb.jpg/600x600bb.jpg”,
“Elton John-Honky Chateau”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/75/b6/da/75b6da76-a3e0-6a77-cbe5-ad6c42cb0692/06UMGIM48050.rgb.jpg/600x600bb.jpg”,
“Elton John-Don’t Shoot Me I’m Only the Piano Player”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/49/cd/b8/49cdb8de-4752-8cec-f795-b7547930990d/06UMGIM34697.rgb.jpg/600x600bb.jpg”,
“Elton John-Caribou”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/08/e8/a9/08e8a97d-1e86-6973-4f69-2093ce8523f8/06UMGIM51740.rgb.jpg/600x600bb.jpg”,
“Elton John-Rock of the Westies”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/f9/91/95/f991955f-ff92-1cc7-9279-ca1d81824d93/00731452816320.rgb.jpg/600x600bb.jpg”,
“Elton John-11-17-70”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/16/d3/d8/16d3d803-8d7f-f0ab-1a7d-3a3c5ad0e3a5/00731452816528.rgb.jpg/600x600bb.jpg”,
“Elton John-Jump Up”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/2d/0c/c5/2d0cc529-841e-9857-d643-ed22371f3e97/00044007711224.rgb.jpg/600x600bb.jpg”,
“Elton John-Too Low for Zero”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/45/e5/b4/45e5b409-0564-a7a3-186c-1dc116c3ffb7/06UMGIM49712.rgb.jpg/600x600bb.jpg”,
“Elton John-Breaking Hearts”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/c8/d0/71/c8d071c5-9373-fe47-9e9c-711053d947cc/00044007711125.rgb.jpg/600x600bb.jpg”,
“Elton John-Reg Strikes Back”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/23/e3/11/23e31147-1b14-ab7c-ac86-d3b321243fe1/00731455847826.rgb.jpg/600x600bb.jpg”,
“Elton John-Greatest Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music122/v4/58/c4/0e/58c40e30-98f8-1215-42f9-82ea7eec6918/22UM1IM16650.rgb.jpg/600x600bb.jpg”,
“Billy Joel-The Stranger”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/37/68/4c/37684c52-dbdf-9bfe-0d87-07492f43dc4c/dj.gmcbwich.jpg/600x600bb.jpg”,
“Billy Joel-52nd Street”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/d6/02/07/d60207ae-a49f-8f20-c8e6-d58e794b9ae5/dj.lzzblgca.jpg/600x600bb.jpg”,
“Billy Joel-Piano Man”: “https://is1-ssl.mzstatic.com/image/thumb/Features125/v4/67/f9/8d/67f98d03-7896-153f-4965-a92458c06965/dj.aiuntmgo.jpg/600x600bb.jpg”,
“Billy Joel-Turnstiles”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/20/9c/8f/209c8fd8-6cdd-6567-74e2-3e62b2803713/888880156822.jpg/600x600bb.jpg”,
“Billy Joel-Glass Houses”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/4a/ff/81/4aff8108-ffe2-e9f4-0259-c02264582603/dj.dhbjhnzy.jpg/600x600bb.jpg”,
“Billy Joel-An Innocent Man”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/b3/ed/8e/b3ed8eb8-e276-ee92-bf69-fba2865516b6/dj.yjsnvawq.jpg/600x600bb.jpg”,
“Billy Joel-Greatest Hits Volume I and II”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/d0/8f/53/d08f535b-f04d-e683-476a-835efc05bf74/13UABIM74016.rgb.jpg/600x600bb.jpg”,
“Michael Jackson-Thriller”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/32/4f/fd/324ffda2-9e51-8f6a-0c2d-c6fd2b41ac55/074643811224.jpg/600x600bb.jpg”,
“Michael Jackson-Off the Wall”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/cc/b0/7f/ccb07f0d-1530-00b0-244a-6332daffc2b9/dj.xnuhftrz.jpg/600x600bb.jpg”,
“Prince-Purple Rain”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/00/17/f2/0017f24f-e580-b77a-71a8-1bc7b75881bf/603497822065.jpg/600x600bb.jpg”,
“The Beatles-Abbey Road”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/48/53/43/485343e3-dd6a-0034-faec-f4b6403f8108/13UMGIM63890.rgb.jpg/600x600bb.jpg”,
“The Beatles-1962-1966”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/7d/1d/99/7d1d9988-88df-70cb-3433-896eda047002/23UM1IM18073.rgb.jpg/600x600bb.jpg”,
“The Beatles-1967-1970”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/a6/8b/65/a68b657c-cac6-68e6-3bde-b79d58fbc795/18UMGIM30762.rgb.jpg/600x600bb.jpg”,
“The Who-Who’s Next”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/a5/e7/70/a5e7703c-4e30-da7a-a319-2d3caef42c0e/23UM1IM04872.rgb.jpg/600x600bb.jpg”,
“The Who-Greatest Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/d3/44/c3/d344c3ab-14ee-5421-a221-45210a741256/078636777429.jpg/600x600bb.jpg”,
“Whitney Houston-Whitney”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/86/b5/25/86b525b1-bff1-4bf6-6112-531251b3d672/dj.hthdmusj.jpg/600x600bb.jpg”,
“Sade-Diamond Life”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/4d/d5/a1/4dd5a1b7-7134-f0ec-b55c-54ac47cc88a5/886448655886.jpg/600x600bb.jpg”,
“Terence Trent D’Arby-Introducing the Hardline”: “https://is1-ssl.mzstatic.com/image/thumb/Music112/v4/50/b1/f3/50b1f3b6-27d7-ec43-b67a-e8f38571f20a/196589157263.jpg/600x600bb.jpg”,
“Tracy Chapman-Tracy Chapman”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/b8/c4/31/b8c431e9-a67d-3917-72e5-be9f5a1ebc46/075596077460.jpg/600x600bb.jpg”,
“Melissa Etheridge-Melissa Etheridge”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/71/e1/25/71e1253a-0547-dcb3-05b4-5e3d6821e671/16UMGIM66600.rgb.jpg/600x600bb.jpg”,
“Christopher Cross-Christopher Cross”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/68/67/7c/68677c9f-8c6c-2c8c-e5a2-4cc4d20dddb7/8721093378327_Cover.jpg/600x600bb.jpg”,
“Rupert Holmes-Partners in Crime”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/3f/d7/3d/3fd73db9-d2dc-9769-8419-67ab5ae4a1f7/06UMGIM03861.rgb.jpg/600x600bb.jpg”,
“The Rolling Stones-Hot Rocks 1964-1971”: “https://is1-ssl.mzstatic.com/image/thumb/Music112/v4/29/b7/81/29b781e5-3773-c87e-c6a8-82581e53d270/13ABKIM00053.rgb.jpg/600x600bb.jpg”,
“The Rolling Stones-Some Girls”: “https://is1-ssl.mzstatic.com/image/thumb/Music113/v4/7d/0c/a9/7d0ca934-e356-086f-2233-d7c0b75a4298/886448272038.jpg/600x600bb.jpg”,
“The Rolling Stones-Tattoo You”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/f6/ce/84/f6ce84ab-f31b-51c6-125f-269a2fdd2639/21UMGIM70501.rgb.jpg/600x600bb.jpg”,
“The Rolling Stones-Emotional Rescue”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/69/64/67/696467fe-9ed1-b65e-1016-a10d07e6464d/20UMGIM14087.rgb.jpg/600x600bb.jpg”,
“The Rolling Stones-Black and Blue”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/90/4b/c3/904bc37d-dfc2-ca38-86f8-bd483798517d/25UM1IM23176.rgb.jpg/600x600bb.jpg”,
“The Rolling Stones-It’s Only Rock n Roll”: “https://is1-ssl.mzstatic.com/image/thumb/Music112/v4/50/b8/2f/50b82fdb-a3db-b284-d323-a243e1abbe34/08UMGIM15718.rgb.jpg/600x600bb.jpg”,
“Rod Stewart-The Rod Stewart Album”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/26/f5/0a/26f50a12-6c89-36d2-578d-d88641ea6e8c/00602547056887.rgb.jpg/600x600bb.jpg”,
“Rod Stewart-A Night on the Town”: “https://is1-ssl.mzstatic.com/image/thumb/Music/49/dc/91/mzi.ldsntdvl.jpg/600x600bb.jpg”,
“Rod Stewart-Foot Loose and Fancy Free”: “https://is1-ssl.mzstatic.com/image/thumb/Music/50/5c/fe/mzi.xgvsdcvd.jpg/600x600bb.jpg”,
“Rod Stewart-Blondes Have More Fun”: “https://is1-ssl.mzstatic.com/image/thumb/Music/7e/9f/37/mzi.uhxzzozn.jpg/600x600bb.jpg”,
“Rod Stewart-Foolish Behaviour”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/1c/bd/71/1cbd712d-0b64-aaa8-8543-d5791cd32c8d/884977897135.jpg/600x600bb.jpg”,
“Rod Stewart-Tonight I’m Yours”: “https://is1-ssl.mzstatic.com/image/thumb/Music/1e/a6/11/mzi.oaqcsgrs.jpg/600x600bb.jpg”,
“Journey-Escape”: “https://is1-ssl.mzstatic.com/image/thumb/Music122/v4/47/d6/fe/47d6fe2f-b14c-d8a7-597c-8a40e094364e/886449932795.jpg/600x600bb.jpg”,
“Journey-Frontiers”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/2a/46/80/2a468026-f788-8bce-3fc8-77976ef2e9d6/074646772324.jpg/600x600bb.jpg”,
“Journey-Departure”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/3d/15/5f/3d155f7d-7eff-c8d8-a167-4b4424254c48/886970011921.jpg/600x600bb.jpg”,
“Journey-Journey”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/ba/2a/6f/ba2a6f4f-d645-c841-d5ce-4d4b50bc930d/074643338820.jpg/600x600bb.jpg”,
“Foreigner-4”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/1b/ba/5c/1bba5c8d-ba79-9b11-3406-0bbddc5bcb97/603497979752.jpg/600x600bb.jpg”,
“Foreigner-Double Vision”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/98/46/0f/98460fb8-9dae-89b7-6196-325187c6a84b/603497979769.jpg/600x600bb.jpg”,
“Foreigner-Head Games”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/cf/99/80/cf998071-0dbb-b47e-303b-456e57d2dafe/603497968275.jpg/600x600bb.jpg”,
“Foreigner-Foreigner”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/5c/7b/1a/5c7b1a52-7749-202f-e49b-5bf71822897f/081227427061.jpg/600x600bb.jpg”,
“REO Speedwagon-Hi Infidelity”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/c0/dd/3c/c0dd3c82-38cd-2f7e-e4d6-53d6519facb4/mzi.akuwasch.jpg/600x600bb.jpg”,
“Toto-Toto IV”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/69/ce/d2/69ced240-07a7-2a04-bbab-2afbacf30809/074643772822.jpg/600x600bb.jpg”,
“Toto-Toto”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/a1/a9/ea/a1a9eaef-2d81-c2c6-9a03-82c3cbc9e598/074643531726.jpg/600x600bb.jpg”,
“Asia-Asia”: “https://is1-ssl.mzstatic.com/image/thumb/Music112/v4/e9/5d/f0/e95df063-5e21-6307-3962-bcfa4fa383e5/06UMGIM08894.rgb.jpg/600x600bb.jpg”,
“Heart-Heart”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/7f/a1/21/7fa12144-00a8-8767-9dab-bffb4018194a/17UMGIM97463.rgb.jpg/600x600bb.jpg”,
“Pat Benatar-Crimes of Passion”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/1e/fe/91/1efe916e-83c5-9b1f-1c93-bc84228f7d1b/13UAEIM58631.rgb.jpg/600x600bb.jpg”,
“Pat Benatar-In the Heat of the Night”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/f8/dd/07/f8dd07be-a662-f705-4da5-cf30ced36c49/13UAEIM58649.rgb.jpg/600x600bb.jpg”,
“Pat Benatar-Precious Time”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/e0/e1/2a/e0e12a05-3ba5-572e-382a-20a3e985e2b7/00602537705702.rgb.jpg/600x600bb.jpg”,
“Pat Benatar-Get Nervous”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/2b/fb/e9/2bfbe997-91b6-148b-c33f-5bc0d5a11f56/00094632139658.jpg/600x600bb.jpg”,
“Pat Benatar-Tropico”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/d3/ea/c1/d3eac152-5d41-5926-eb5f-a8c6402415a9/13UABIM52087.rgb.jpg/600x600bb.jpg”,
“Huey Lewis and the News-Sports”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/c6/25/81/c625810f-e0db-3a3e-c595-dee7c959a68a/13UMGIM47963.rgb.jpg/600x600bb.jpg”,
“Huey Lewis and the News-Fore!”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/b0/7f/cc/b07fcce9-0d39-9ab0-e1dd-54ffd1642db6/13UABIM36878.rgb.jpg/600x600bb.jpg”,
“Huey Lewis and the News-Small World”: “https://is1-ssl.mzstatic.com/image/thumb/Music4/v4/66/4e/e2/664ee2bf-0146-c370-1751-3bb6b6971cf1/00094632162250.jpg/600x600bb.jpg”,
“Don Henley-Building the Perfect Beast”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/b7/e5/23/b7e52326-6c54-d20b-8640-02dc563d0e87/24UMGIM83915.rgb.jpg/600x600bb.jpg”,
“Rick Springfield-Working Class Dog”: “https://is1-ssl.mzstatic.com/image/thumb/Music113/v4/52/1e/e8/521ee88b-8fcc-0d53-345d-3b82d910a554/828768486726.jpg/600x600bb.jpg”,
“Hall and Oates-Voices”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/e0/96/ad/e096ad2e-7ec8-480e-8ddf-d9aa469d4610/828765861427.jpg/600x600bb.jpg”,
“Chicago-Chicago IX Greatest Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/22/17/ae/2217ae2e-99d0-7f34-c84f-ae48f9439feb/603497878406.jpg/600x600bb.jpg”,
“Cheap Trick-At Budokan”: “https://is1-ssl.mzstatic.com/image/thumb/Music1/v4/bc/6a/4e/bc6a4e31-f031-45a6-8def-159a28c76940/dj.lrtmcofv.jpg/600x600bb.jpg”,
“Cheap Trick-Dream Police”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/0f/89/89/0f898981-77f3-b37d-5b4e-de8a3f7be3fd/074643577328.jpg/600x600bb.jpg”,
“Cheap Trick-Lap of Luxury”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/f5/00/f6/f500f601-9ab6-ed11-262e-361dfc5228da/dj.tcqtkulq.jpg/600x600bb.jpg”,
“Queen-Greatest Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/4d/08/2a/4d082a9e-7898-1aa1-a02f-339810058d9e/14DMGIM05632.rgb.jpg/600x600bb.jpg”,
“Queen-The Game”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/65/a6/d1/65a6d100-9e4f-d9a3-84ed-cf8777729f56/14DMGIM05552.rgb.jpg/600x600bb.jpg”,
“Steve Perry-Street Talk”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/10/df/9c/10df9c90-8ae9-14c0-547f-bd59e6b275d1/888880070920.jpg/600x600bb.jpg”,
“Steve Winwood-Arc of a Diver”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/35/cd/9a/35cd9aa5-e3c3-cd22-b051-a52d57ab34c8/00602567850502.rgb.jpg/600x600bb.jpg”,
“Steve Winwood-Roll With It”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/8d/d8/f7/8dd8f70a-e345-3483-67e9-4c618a39d39c/00077778606956.jpg/600x600bb.jpg”,
“Sting-The Dream of the Blue Turtles”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/e2/a4/a2/e2a4a265-f581-e336-5276-36fd694f9217/25UMGIM57592.rgb.jpg/600x600bb.jpg”,
“John Lennon-Double Fantasy”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/67/f1/4d/67f14daa-7b2d-814c-d301-8b4c928e633e/16UMGIM17510.rgb.jpg/600x600bb.jpg”,
“Wings-Wings Over America”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/c3/54/7f/c3547f47-a087-50f3-76f2-b3b6aa1fe6a6/13CMGIM00038.rgb.jpg/600x600bb.jpg”,
“Bryan Adams-Reckless”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/67/10/6b/67106b63-8edc-ef66-78f2-ad85b275074f/06UMGIM28735.rgb.jpg/600x600bb.jpg”,
“Bryan Adams-Cuts Like a Knife”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/f2/68/69/f268697b-d1a7-395e-9a7d-26d3571b473b/00075021328822.rgb.jpg/600x600bb.jpg”,
“Bryan Adams-You Want It You Got It”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/10/36/e8/1036e87f-9f68-5226-db56-7f1346a9fcdd/00075021315426.rgb.jpg/600x600bb.jpg”,
“Bryan Adams-Bryan Adams”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/55/4a/4a/554a4a0b-ec28-e89a-6611-25d5b16ace9b/00082839310024.rgb.jpg/600x600bb.jpg”,
“Loverboy-Get Lucky”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/ab/c1/03/abc10396-1a32-2b95-4166-68f4850d8f2f/196873103181.jpg/600x600bb.jpg”,
“Loverboy-Keep It Up”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/9f/bb/af/9fbbaff1-e586-5bda-3617-224f2f33ad09/074643870320.jpg/600x600bb.jpg”,
“Loverboy-Loverboy”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/38/41/7d/38417d24-9459-bab5-2d41-da44d7946b3d/1836.jpg/600x600bb.jpg”,
“April Wine-The Nature of the Beast”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/91/45/cf/9145cf98-fd27-04ab-9698-bb704685d356/5400863132118_cover.jpg/600x600bb.jpg”,
“April Wine-Power Play”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/c3/4b/3e/c34b3e0c-4e50-b26f-62f0-9ed9f0182608/5400863132101_cover.jpg/600x600bb.jpg”,
“April Wine-Harder Faster”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/68/4e/b6/684eb6fe-97f0-4b71-fc2b-a16192f943a3/5400863132125_cover.jpg/600x600bb.jpg”,
“April Wine-Greatest Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/0e/ce/c2/0ecec25f-30e7-6c09-e0a8-d2755103bc7c/1834.jpg/600x600bb.jpg”,
“Corey Hart-Boy in the Box”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/61/fd/85/61fd85c3-332e-cc2e-cadc-7795172de586/00602547865267.rgb.jpg/600x600bb.jpg”,
“Corey Hart-First Offense”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/fc/63/63/fc636333-9781-6772-9f2a-41ee9e3d9fdd/00602547865243.rgb.jpg/600x600bb.jpg”,
“Glass Tiger-The Thin Red Line”: “https://is1-ssl.mzstatic.com/image/thumb/Music7/v4/1c/60/5c/1c605cb6-6792-7ad6-8fe1-53c036ef4bc7/05099963653953.jpg/600x600bb.jpg”,
“Trooper-Hot Shots”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/91/74/97/917497ac-a7a5-a1da-ceba-e3cdb17bbb95/20UMGIM67519.rgb.jpg/600x600bb.jpg”,
“Prism-The Best of Prism”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/01/a8/47/01a84725-3df4-4ecd-eea2-8c13c74118d0/522c5012-9cb1-4852-96c9-8152b018bc28.jpg/600x600bb.jpg”,
“Bachman-Turner Overdrive-Not Fragile”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/66/3c/c8/663cc8bc-fc9a-9524-6b81-eb863865b5b4/06UMGIM53974.rgb.jpg/600x600bb.jpg”,
“Bachman-Turner Overdrive-Four Wheel Drive”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/4c/cf/6a/4ccf6aa7-522a-8743-cfe6-70a2ffe988bb/06UMGIM03500.rgb.jpg/600x600bb.jpg”,
“Burton Cummings-Dream of a Child”: “https://is1-ssl.mzstatic.com/image/thumb/Music/d4/47/63/mzi.jpjvdeso.jpg/600x600bb.jpg”,
“Headpins-Line of Fire”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/61/10/ed/6110ed71-eb5c-ba6d-542e-22cdc755aad1/696774101120_cover.jpg/600x600bb.jpg”,
“Harlequin-Love Crimes”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/33/ad/30/33ad3090-c64e-50bc-8bd1-fddb1226b908/886448417217.jpg/600x600bb.jpg”,
“Toronto-Girls Night Out”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/a7/89/86/a78986c1-f1d0-bb2c-57a4-a438f210ab43/696774102028_cover.jpg/600x600bb.jpg”,
“Toronto-Greatest Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/d3/44/c3/d344c3ab-14ee-5421-a221-45210a741256/078636777429.jpg/600x600bb.jpg”,
“Streetheart-Streetheart”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/3d/cb/5a/3dcb5a06-659d-d8fc-7328-bc3fef229e75/KOpaxNbxDGL9x-streetheart-original-copy-ea272909.png/600x600bb.jpg”,
“Spoons-Talkback”: “https://is1-ssl.mzstatic.com/image/thumb/Music/b5/b7/59/mzi.kopdnejo.jpg/600x600bb.jpg”,
“Boys Brigade-Boys Brigade”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/93/80/2e/93802e0a-0b74-adb4-9eb1-0d62e066d292/22UMGIM14385.rgb.jpg/600x600bb.jpg”,
“Rough Trade-Weapons”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/64/fd/70/64fd70ca-0a7c-c1ee-0e29-a911b7f289cd/620638047521_cover.jpg/600x600bb.jpg”,
“Luba-Secrets and Sins”: “https://is1-ssl.mzstatic.com/image/thumb/Music113/v4/42/5a/c6/425ac65c-848e-3ab1-aa71-0f8ca622e975/19UM1IM09442.rgb.jpg/600x600bb.jpg”,
“Powder Blues Band-Thirsty Ears”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/c2/65/2e/c2652e6d-812d-80f6-c346-7a7fff95c5b4/mzi.sjhtiykk.jpg/600x600bb.jpg”,
“Queen City Kids-Black Box”: “https://is1-ssl.mzstatic.com/image/thumb/Music113/v4/7e/2b/f4/7e2bf4cf-ad01-e20e-b812-52cde41bda70/4511820-97286.jpg/600x600bb.jpg”,
“Doucette-Mama Let Him Play”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/aa/d1/4e/aad14e4c-6e8f-fde5-2f0c-89841c076a1f/803057018024_cover.jpg/600x600bb.jpg”,
“Anne Murray-A Little Good News”: “https://is1-ssl.mzstatic.com/image/thumb/Music6/v4/cf/c4/22/cfc422c5-a1ad-7828-d0e1-a312f2c43125/05099951405458.jpg/600x600bb.jpg”,
“Alannah Myles-Alannah Myles”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/af/ac/b1/afacb116-c6e9-ae05-a3e5-efdd6ded7094/199538722796.jpg/600x600bb.jpg”,
“Northern Lights-Tears Are Not Enough”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/43/02/74/43027407-3cbd-a3ec-09e5-ce9f5c202244/19UMGIM64276.rgb.jpg/600x600bb.jpg”,
“Diane Dufresne-Strip Tease”: “https://is1-ssl.mzstatic.com/image/thumb/Music127/v4/0a/a0/64/0aa064dc-1156-ea75-96b5-5be7e169d94f/1979_Striptease_3000x3000.jpg/600x600bb.jpg”,
“Bob and Doug McKenzie-Great White North”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/f0/77/af/f077af3f-bb11-cfcf-08c5-2566836a1a41/00602547320797.rgb.jpg/600x600bb.jpg”,
“Steve Martin-Let’s Get Small”: “https://is1-ssl.mzstatic.com/image/thumb/Music/89/37/2f/mzi.jmiynxvy.jpg/600x600bb.jpg”,
“Steve Martin-A Wild and Crazy Guy”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/68/b8/41/68b84151-dc2c-8796-1115-2d64796cda1d/s06.upmxyftc.jpg/600x600bb.jpg”,
“Various Artists-Saturday Night Fever”: “https://is1-ssl.mzstatic.com/image/thumb/Music113/v4/74/c1/73/74c17380-b75d-4d74-31d8-99f8e8b20c36/17UM1IM27905.rgb.jpg/600x600bb.jpg”,
“Various Artists-Flashdance”: “https://is1-ssl.mzstatic.com/image/thumb/Music122/v4/40/97/5f/40975fe8-20e6-e617-7e1c-c8dc1dbc753a/06UMGIM29411.rgb.jpg/600x600bb.jpg”,
“Various Artists-Footloose”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/ab/e1/f9/abe1f91c-4f32-4384-6632-13334c07c4b2/886446159812.jpg/600x600bb.jpg”,
“Various Artists-The Rocky Horror Picture Show”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/de/e3/49/dee3493a-14c8-0ded-8e27-05561bf8cc93/00050087362898.rgb.jpg/600x600bb.jpg”,
“Various Artists-The Big Chill”: “https://is1-ssl.mzstatic.com/image/thumb/Music113/v4/9f/e7/f9/9fe7f9f4-ba2f-b369-707f-76899faf1f2c/06UMGIM07593.rgb.jpg/600x600bb.jpg”,
“Various Artists-More Songs from The Big Chill”: “https://is1-ssl.mzstatic.com/image/thumb/Music122/v4/2a/0e/ab/2a0eab6a-9fc2-3945-9827-882f5c64b022/06UMGIM01189.rgb.jpg/600x600bb.jpg”,
“Various Artists-Stand by Me”: “https://is1-ssl.mzstatic.com/image/thumb/Music/82/f1/87/mzi.lvooiexk.jpg/600x600bb.jpg”,
“Various Artists-American Graffiti”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/81/2f/27/812f27f8-4c88-7b16-8a54-5113fa169781/dj.jvcruggi.jpg/600x600bb.jpg”,
“Various Artists-Woodstock”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/26/5c/37/265c3733-e0a5-5758-6de0-f6a9f2a1ad6d/06PNDIM00838.rgb.jpg/600x600bb.jpg”,
“Various Artists-Weird Science”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/ce/80/89/ce808921-d594-fa7f-03e8-adcc92a69de8/06PNDIM00020.rgb.jpg/600x600bb.jpg”,
“Various Artists-Eddie and the Cruisers”: “https://is1-ssl.mzstatic.com/image/thumb/Features115/v4/49/21/4d/49214d77-2eb5-60a1-75d3-67d30b449326/dj.szkhyuyj.jpg/600x600bb.jpg”,
“Various Artists-Against All Odds”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/77/55/e7/7755e7e1-3760-1017-4805-7661bf7995c7/195081003436.jpg/600x600bb.jpg”,
“B.B. King-Live in Cook County Jail”: “https://is1-ssl.mzstatic.com/image/thumb/Music118/v4/1f/8e/4c/1f8e4c35-2d0b-52e0-3ae8-f32314b3a2c7/00602547482358.rgb.jpg/600x600bb.jpg”,
“B.B. King-The Best of B.B. King”: “https://is1-ssl.mzstatic.com/image/thumb/Music/v4/f3/9d/b8/f39db882-f73a-9ccf-9665-d184e0123d6f/00077778623052.jpg/600x600bb.jpg”,
“B.B. King-The Incredible Soul of B.B. King”: “https://is1-ssl.mzstatic.com/image/thumb/Music112/v4/ad/ce/9f/adce9fb5-6eb1-e7d0-1f87-26a474376024/15UMGIM54243.rgb.jpg/600x600bb.jpg”,
“B.B. King-Midnight Believer”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/72/3b/7c/723b7c87-73c2-cb3b-8d4e-6532fb39e4c6/00076742701123.rgb.jpg/600x600bb.jpg”,
“Bob Marley-Legend”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/3c/c2/0d/3cc20dcc-8f4e-f060-36dd-7de52a7ec8fe/12UMGIM14712.rgb.jpg/600x600bb.jpg”,
“Peter Tosh-Legalize It”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/2b/29/60/2b2960f7-ca5b-ab5b-9619-85d39746d8f7/dj.awruiyqo.jpg/600x600bb.jpg”,
“Peter Tosh-Equal Rights”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/cf/31/9a/cf319a45-3025-e8ec-4a62-c9545038aaf0/886445304053.jpg/600x600bb.jpg”,
“UB40-Labour of Love”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/84/c2/52/84c25239-f4a6-1b09-c046-9669784497d6/00077778641254.rgb.jpg/600x600bb.jpg”,
“UB40-Geffery Morgan”: “https://is1-ssl.mzstatic.com/image/thumb/Music/v4/8d/b6/e4/8db6e47e-c2c1-eae8-9c7c-c02ff9bd9b89/00077778644453.jpg/600x600bb.jpg”,
“Commodores-Midnight Magic”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/f2/0c/35/f20c3506-acea-18cb-e39a-233b37d51d65/00602547309389.rgb.jpg/600x600bb.jpg”,
“Commodores-Greatest Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/e3/33/d5/e333d5f5-fb00-839a-8265-c6eea35e2217/06UMGIM75575.rgb.jpg/600x600bb.jpg”,
“U2-The Joshua Tree”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/8f/e2/c3/8fe2c384-f6cb-9af7-371d-2b6a9b204e59/17UMGIM79292.rgb.jpg/600x600bb.jpg”,
“U2-War”: “https://is1-ssl.mzstatic.com/image/thumb/Music221/v4/6b/0a/0d/6b0a0d5b-2831-06e9-3471-3befad197c1d/17UMGIM87714.rgb.jpg/600x600bb.jpg”,
“U2-The Unforgettable Fire”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/40/45/51/40455196-536d-d9a2-c8a7-4129ce944a67/17UMGIM94892.rgb.jpg/600x600bb.jpg”,
“U2-Under a Blood Red Sky”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/25/17/47/25174760-2463-29c8-8015-59dce91ee674/17UMGIM87705.rgb.jpg/600x600bb.jpg”,
“Midnight Oil-Diesel and Dust”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/1d/ae/99/1dae999a-8d53-a45a-d14e-7e811c752b12/886971827323.jpg/600x600bb.jpg”,
“Dire Straits-Brothers in Arms”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/e8/bd/54/e8bd54fd-6595-4ebf-aa4c-58139ed316e6/dj.bngafruf.jpg/600x600bb.jpg”,
“The Pretenders-Learning to Crawl”: “https://is1-ssl.mzstatic.com/image/thumb/Music/3a/38/f9/mzi.tiwmdwvb.jpg/600x600bb.jpg”,
“Elvis Costello-The Best of Elvis Costello”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/1d/e0/20/1de0208c-9781-b10e-d92d-51db8387dc36/00602517260917.rgb.jpg/600x600bb.jpg”,
“Elvis Costello-Punch the Clock”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/54/67/92/546792bf-d6a5-e416-8f11-88bdafb3514d/00602547382214.rgb.jpg/600x600bb.jpg”,
“Joe Jackson-Look Sharp!”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/a0/a1/09/a0a10910-c7ad-26b1-666e-be734b94c41e/00731458619420.rgb.jpg/600x600bb.jpg”,
“Joe Jackson-I’m the Man”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/de/42/c9/de42c93c-2a79-e766-bb1f-23ccf70bb8ff/00606949308926.rgb.jpg/600x600bb.jpg”,
“Joe Jackson-Body and Soul”: “https://is1-ssl.mzstatic.com/image/thumb/Music128/v4/22/08/74/220874c3-d901-c880-1c0e-f1ad0dbdb9b8/00082839500029.rgb.jpg/600x600bb.jpg”,
“The Kinks-Give the People What They Want”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/73/4f/e8/734fe82a-3b39-6c53-c9f0-63afe925cb27/4050538636819.jpg/600x600bb.jpg”,
“The Kinks-State of Confusion”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/a9/b3/41/a9b34172-b26b-65cc-c400-534c3c9d52e8/4050538636925.jpg/600x600bb.jpg”,
“The Kinks-One for the Road”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/b4/9b/65/b49b65fd-573e-ba7a-e374-37beb9eb5738/4050538636857.jpg/600x600bb.jpg”,
“Los Lobos-How Will the Wolf Survive”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/57/da/73/57da735e-3512-c0a8-0b94-356d4a76820a/mzi.bclvaczz.jpg/600x600bb.jpg”,
“Bob Seger-Night Moves”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/6f/99/f7/6f99f718-71d6-c728-eb53-b1218c41195b/14UMGIM36915.rgb.jpg/600x600bb.jpg”,
“Bob Seger-Against the Wind”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/bc/2b/a7/bc2ba744-50e7-284d-8ea9-8d643f95d9f7/17UM1IM02542.rgb.jpg/600x600bb.jpg”,
“Bob Seger-Nine Tonight”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/eb/12/e9/eb12e9f6-14a5-1e95-206b-349d5f1a1aed/00602557774740.rgb.jpg/600x600bb.jpg”,
“John Cougar-American Fool”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/7c/a8/38/7ca83867-fed2-55cb-be03-73a182100b00/06UMGIM12400.rgb.jpg/600x600bb.jpg”,
“John Cougar Mellencamp-Uh-Huh”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/49/c7/2e/49c72e5e-05ec-0ed1-d88f-f43a2b6e86d5/06UMGIM12397.rgb.jpg/600x600bb.jpg”,
“John Fogerty-Centerfield”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/36/51/62/36516278-bdca-b907-1d4e-7558b4bdf9d8/25CRGIM51047.rgb.jpg/600x600bb.jpg”,
“John Cafferty-Tough All Over”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/ac/09/c0/ac09c0d0-3969-3cac-cc95-712cce6af64b/dj.hrvrfeiy.jpg/600x600bb.jpg”,
“Jackson Browne-Running on Empty”: “https://is1-ssl.mzstatic.com/image/thumb/Music113/v4/95/f7/f3/95f7f3a3-d793-fb71-572d-4dbd686e5252/603497851195.jpg/600x600bb.jpg”,
“Jackson Browne-Hold Out”: “https://is1-ssl.mzstatic.com/image/thumb/Features124/v4/e8/27/c8/e827c834-2c0e-ef4c-2b71-3f83d28751d7/dj.zafjlyme.jpg/600x600bb.jpg”,
“Lynyrd Skynyrd-Gold and Platinum”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/57/00/f3/5700f331-2d06-7f5d-cb98-43970fd52874/14UMGIM00860.rgb.jpg/600x600bb.jpg”,
“Steve Miller Band-Fly Like an Eagle”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/58/bc/73/58bc732d-8566-9626-f8da-2d9e54a3356e/13UMGIM43075.rgb.jpg/600x600bb.jpg”,
“Steve Miller Band-Greatest Hits 1974-78”: “https://is1-ssl.mzstatic.com/image/thumb/Music112/v4/78/f8/cd/78f8cdac-35ff-34ee-4303-f447ef3a2c3f/13UAAIM00691.rgb.jpg/600x600bb.jpg”,
“Air Supply-Lost in Love”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/e7/e8/d8/e7e8d87f-a62c-44c5-b698-6698ff04c919/4007192590452.jpg/600x600bb.jpg”,
“Air Supply-The One That You Love”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/c6/60/b8/c660b830-b594-25f8-2da1-8378120eed7b/744659977725.jpg/600x600bb.jpg”,
“Al Stewart-Year of the Cat”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/1e/f7/8f/1ef78f79-57f6-e66f-b693-13e1ab55c840/5059460078529.jpg/600x600bb.jpg”,
“Chris de Burgh-Spanish Train and Other Stories”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/2f/e0/6b/2fe06bb4-a37b-6042-2492-196000fbe363/00602567778417.rgb.jpg/600x600bb.jpg”,
“Cliff Richard-I’m No Hero”: “https://is1-ssl.mzstatic.com/image/thumb/Music123/v4/1f/41/90/1f4190dd-cd5c-66b5-e918-ca1f0811e3cc/724353311353.jpg/600x600bb.jpg”,
“Dr. Hook-Greatest Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/8c/a1/ea/8ca1ea78-cccf-6423-631e-23b880cb33f2/13UABIM57230.rgb.jpg/600x600bb.jpg”,
“Dr. Hook-Pleasure and Pain”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/9b/fe/90/9bfe9007-9218-92b2-2763-1e46a038755a/00724383820955.jpg/600x600bb.jpg”,
“Juice Newton-Juice”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/78/a6/59/78a65979-c0a5-ee3f-aa89-3e39f269254d/00602547853707.rgb.jpg/600x600bb.jpg”,
“Little River Band-Sleeper Catcher”: “https://is1-ssl.mzstatic.com/image/thumb/Music122/v4/c9/af/4c/c9af4c89-17d5-d587-393d-1d6a071ecead/22UM1IM08564.rgb.jpg/600x600bb.jpg”,
“Bee Gees-Spirits Having Flown”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/eb/c1/2a/ebc12a2f-43a8-e4f7-08ab-c75c1cd778e7/06UMGIM67709.rgb.jpg/600x600bb.jpg”,
“Bee Gees-Main Course”: “https://is1-ssl.mzstatic.com/image/thumb/Music123/v4/bb/15/72/bb157228-9c5f-c368-e952-cb94e80ffd69/06UMGIM31816.rgb.jpg/600x600bb.jpg”,
“Village People-Go West”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/35/d7/2f/35d72f8f-0931-d530-c6e9-1df547cda6c2/00731453217225.rgb.jpg/600x600bb.jpg”,
“Steely Dan-Gold”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/a8/cf/ee/a8cfeee3-d94f-6ec7-ab13-d4033937f201/06UMGIM03449.rgb.jpg/600x600bb.jpg”,
“Simon and Garfunkel-Greatest Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/7c/0e/db/7c0edb31-3d24-c264-abaa-411f862cdcf7/886447698259.jpg/600x600bb.jpg”,
“Jay Ferguson-Thunder Island”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/e1/0d/fa/e10dfa0e-4bc3-4222-c882-a605516ad509/mzi.uxstxvkd.jpg/600x600bb.jpg”,
“Stray Cats-Built for Speed”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/b7/5e/1b/b75e1b45-bee4-40ae-e82e-3eabedb8e0cf/00602537813889.rgb.jpg/600x600bb.jpg”,
“Stray Cats-Rant N Rave with the Stray Cats”: “https://is1-ssl.mzstatic.com/image/thumb/Music118/v4/3a/d4/43/3ad44397-c980-f42a-9a8b-d4fff48fbbce/00602557741308.rgb.jpg/600x600bb.jpg”,
“The Blasters-The Blasters”: “https://is1-ssl.mzstatic.com/image/thumb/Music/f3/b4/62/mzi.fzrgzoss.jpg/600x600bb.jpg”,
“The Blasters-Non-Fiction”: “https://is1-ssl.mzstatic.com/image/thumb/Music211/v4/9c/4a/83/9c4a8399-a970-ff34-70d9-103c4ef402a7/810177211822.png/600x600bb.jpg”,
“Chuck Berry-Golden Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music116/v4/74/86/9f/74869f4f-c52b-289b-31a7-9ac9fec31e2d/06UMGIM70785.rgb.jpg/600x600bb.jpg”,
“Jerry Lee Lewis-Greatest Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/61/bd/20/61bd203e-59a8-b992-e422-56085a628a0d/21UMGIM65825.rgb.jpg/600x600bb.jpg”,
“Jan and Dean-Greatest Hits”: “https://is1-ssl.mzstatic.com/image/thumb/Music123/v4/76/8f/c6/768fc622-9007-dc63-65fd-04789e8ca144/05099994722758.rgb.jpg/600x600bb.jpg”,
“Alabama-Mountain Music”: “https://is1-ssl.mzstatic.com/image/thumb/Music124/v4/7a/84/cc/7a84cc1f-593b-6887-216c-707721c825a2/078635422924.jpg/600x600bb.jpg”,
“Alabama-The Closer You Get”: “https://is1-ssl.mzstatic.com/image/thumb/Music125/v4/75/4d/43/754d43b9-bddf-e5b0-5c26-b556c79f31cb/dj.obznxalu.jpg/600x600bb.jpg”,
“Merle Haggard-Let Me Tell You About a Song”: “https://is1-ssl.mzstatic.com/image/thumb/Music/v4/14/c2/59/14c259a3-0ab6-7599-0ef6-f85c38e4d328/05099964251554.jpg/600x600bb.jpg”,
“Various Artists-Reggae Greats”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/c3/55/d8/c355d8ca-1b56-e758-7fc2-f27707d8e9cc/00050087368944.rgb.jpg/600x600bb.jpg”,
“Various Artists-History of U.S. Rock”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/4c/5d/31/4c5d3166-3a4b-fb3c-473e-1776ff947466/13DMGIM04437.rgb.jpg/600x600bb.jpg”,
“Various Artists-Stars on Long Play”: “https://is1-ssl.mzstatic.com/image/thumb/Music126/v4/4c/5d/31/4c5d3166-3a4b-fb3c-473e-1776ff947466/13DMGIM04437.rgb.jpg/600x600bb.jpg”,
“Various Artists-Pure Gold Collection”: “https://is1-ssl.mzstatic.com/image/thumb/Music115/v4/2d/7e/23/2d7e2381-2557-ba95-b30f-505f18fb3a97/886444863070.jpg/600x600bb.jpg”,
“Various Artists-Get It On”: “https://is1-ssl.mzstatic.com/image/thumb/Music114/v4/c3/55/d8/c355d8ca-1b56-e758-7fc2-f27707d8e9cc/00050087368944.rgb.jpg/600x600bb.jpg”,
“Band Aid-Do They Know It’s Christmas”: “https://is1-ssl.mzstatic.com/image/thumb/Music5/v4/58/71/fc/5871fc45-fac8-df0c-a4de-1d13c02b27b2/14UMGIM56130.jpg/600x600bb.jpg”,
};

```
const vinylData = [
  { artist: "AC/DC", album: "Back in Black", year: 1980, genre: "Hard Rock" },
  { artist: "AC/DC", album: "Highway to Hell", year: 1979, genre: "Hard Rock" },
  { artist: "AC/DC", album: "For Those About to Rock", year: 1981, genre: "Hard Rock" },
  { artist: "AC/DC", album: "Dirty Deeds Done Dirt Cheap", year: 1976, genre: "Hard Rock" },
  { artist: "Led Zeppelin", album: "Physical Graffiti", year: 1975, genre: "Hard Rock" },
  { artist: "Led Zeppelin", album: "Led Zeppelin IV", year: 1971, genre: "Hard Rock" },
  { artist: "Led Zeppelin", album: "In Through the Out Door", year: 1979, genre: "Hard Rock" },
  { artist: "Van Halen", album: "Van Halen", year: 1978, genre: "Hard Rock" },
  { artist: "Def Leppard", album: "Pyromania", year: 1983, genre: "Hard Rock" },
  { artist: "Iron Maiden", album: "The Number of the Beast", year: 1982, genre: "Heavy Metal" },
  { artist: "Ozzy Osbourne", album: "Diary of a Madman", year: 1981, genre: "Heavy Metal" },
  { artist: "Scorpions", album: "Blackout", year: 1982, genre: "Hard Rock" },
  { artist: "Meat Loaf", album: "Bat Out of Hell", year: 1977, genre: "Rock" },
  { artist: "Aerosmith", album: "Night in the Ruts", year: 1979, genre: "Hard Rock" },
  { artist: "Alice Cooper", album: "School's Out", year: 1972, genre: "Hard Rock" },
  { artist: "Alice Cooper", album: "Billion Dollar Babies", year: 1973, genre: "Hard Rock" },
  { artist: "Alice Cooper", album: "Welcome to My Nightmare", year: 1975, genre: "Rock" },
  { artist: "Alice Cooper", album: "Goes to Hell", year: 1976, genre: "Rock" },
  { artist: "Bad Company", album: "Bad Company", year: 1974, genre: "Hard Rock" },
  { artist: "Bad Company", album: "Straight Shooter", year: 1975, genre: "Hard Rock" },
  { artist: "Grand Funk Railroad", album: "All the Girls in the World Beware", year: 1974, genre: "Hard Rock" },
  { artist: "Grand Funk Railroad", album: "Caught in the Act", year: 1975, genre: "Hard Rock" },
  { artist: "Joan Jett and the Blackhearts", album: "I Love Rock n Roll", year: 1981, genre: "Hard Rock" },
  { artist: "Aldo Nova", album: "Aldo Nova", year: 1982, genre: "Hard Rock" },
  { artist: "Billy Squier", album: "Don't Say No", year: 1981, genre: "Hard Rock" },
  { artist: "Billy Squier", album: "Emotions in Motion", year: 1982, genre: "Hard Rock" },
  { artist: "Night Ranger", album: "Dawn Patrol", year: 1982, genre: "Hard Rock" },
  { artist: "New England", album: "New England", year: 1979, genre: "Hard Rock" },
  { artist: "Kiss", album: "Destroyer", year: 1976, genre: "Hard Rock" },
  { artist: "Kiss", album: "Rock and Roll Over", year: 1976, genre: "Hard Rock" },
  { artist: "Kiss", album: "Love Gun", year: 1977, genre: "Hard Rock" },
  { artist: "Kiss", album: "Alive!", year: 1975, genre: "Hard Rock" },
  { artist: "Kiss", album: "Alive II", year: 1977, genre: "Hard Rock" },
  { artist: "Kiss", album: "Dynasty", year: 1979, genre: "Hard Rock" },
  { artist: "Kiss", album: "The Originals", year: 1976, genre: "Hard Rock" },
  { artist: "Ace Frehley", album: "Ace Frehley", year: 1978, genre: "Hard Rock" },
  { artist: "Gene Simmons", album: "Gene Simmons", year: 1978, genre: "Hard Rock" },
  { artist: "Paul Stanley", album: "Paul Stanley", year: 1978, genre: "Hard Rock" },
  { artist: "Peter Criss", album: "Peter Criss", year: 1978, genre: "Hard Rock" },
  { artist: "Pink Floyd", album: "The Dark Side of the Moon", year: 1973, genre: "Progressive Rock" },
  { artist: "Pink Floyd", album: "The Wall", year: 1979, genre: "Progressive Rock" },
  { artist: "Rush", album: "Moving Pictures", year: 1981, genre: "Progressive Rock" },
  { artist: "Rush", album: "Hemispheres", year: 1978, genre: "Progressive Rock" },
  { artist: "Yes", album: "90125", year: 1983, genre: "Progressive Rock" },
  { artist: "Genesis", album: "Abacab", year: 1981, genre: "Progressive Rock" },
  { artist: "Genesis", album: "Genesis", year: 1983, genre: "Progressive Rock" },
  { artist: "Supertramp", album: "Breakfast in America", year: 1979, genre: "Progressive Rock" },
  { artist: "Supertramp", album: "Crime of the Century", year: 1974, genre: "Progressive Rock" },
  { artist: "Supertramp", album: "Even in the Quietest Moments", year: 1977, genre: "Progressive Rock" },
  { artist: "Styx", album: "Paradise Theatre", year: 1981, genre: "Progressive Rock" },
  { artist: "Styx", album: "The Grand Illusion", year: 1977, genre: "Progressive Rock" },
  { artist: "Styx", album: "Pieces of Eight", year: 1978, genre: "Progressive Rock" },
  { artist: "Styx", album: "Cornerstone", year: 1979, genre: "Progressive Rock" },
  { artist: "Styx", album: "Crystal Ball", year: 1976, genre: "Progressive Rock" },
  { artist: "Styx", album: "Equinox", year: 1975, genre: "Progressive Rock" },
  { artist: "The Alan Parsons Project", album: "Eye in the Sky", year: 1982, genre: "Progressive Rock" },
  { artist: "The Alan Parsons Project", album: "I Robot", year: 1977, genre: "Progressive Rock" },
  { artist: "The Moody Blues", album: "Long Distance Voyager", year: 1981, genre: "Progressive Rock" },
  { artist: "Electric Light Orchestra", album: "Out of the Blue", year: 1977, genre: "Progressive Rock" },
  { artist: "Electric Light Orchestra", album: "ELO's Greatest Hits", year: 1979, genre: "Progressive Rock" },
  { artist: "Electric Light Orchestra", album: "Ole ELO", year: 1976, genre: "Progressive Rock" },
  { artist: "The Police", album: "Synchronicity", year: 1983, genre: "New Wave" },
  { artist: "The Police", album: "Ghost in the Machine", year: 1981, genre: "New Wave" },
  { artist: "The Police", album: "Zenyatta Mondatta", year: 1980, genre: "New Wave" },
  { artist: "Duran Duran", album: "Rio", year: 1982, genre: "New Wave" },
  { artist: "Duran Duran", album: "Seven and the Ragged Tiger", year: 1983, genre: "New Wave" },
  { artist: "Duran Duran", album: "Duran Duran", year: 1981, genre: "New Wave" },
  { artist: "Eurythmics", album: "Sweet Dreams Are Made of This", year: 1983, genre: "Synth-Pop" },
  { artist: "Eurythmics", album: "Touch", year: 1983, genre: "Synth-Pop" },
  { artist: "Eurythmics", album: "1984", year: 1984, genre: "Synth-Pop" },
  { artist: "Tears for Fears", album: "Songs from the Big Chair", year: 1985, genre: "Synth-Pop" },
  { artist: "Tears for Fears", album: "The Hurting", year: 1983, genre: "Synth-Pop" },
  { artist: "The Cars", album: "Heartbeat City", year: 1984, genre: "New Wave" },
  { artist: "The Cars", album: "The Cars", year: 1978, genre: "New Wave" },
  { artist: "The Cars", album: "Candy-O", year: 1979, genre: "New Wave" },
  { artist: "The Cars", album: "Shake It Up", year: 1981, genre: "New Wave" },
  { artist: "Culture Club", album: "Colour by Numbers", year: 1983, genre: "New Wave" },
  { artist: "Culture Club", album: "Kissing to Be Clever", year: 1982, genre: "New Wave" },
  { artist: "Culture Club", album: "Waking Up with the House on Fire", year: 1984, genre: "New Wave" },
  { artist: "Billy Idol", album: "Rebel Yell", year: 1983, genre: "New Wave" },
  { artist: "Billy Idol", album: "Billy Idol", year: 1982, genre: "New Wave" },
  { artist: "A Flock of Seagulls", album: "Listen", year: 1983, genre: "Synth-Pop" },
  { artist: "Thompson Twins", album: "Into the Gap", year: 1984, genre: "Synth-Pop" },
  { artist: "Spandau Ballet", album: "True", year: 1983, genre: "New Wave" },
  { artist: "INXS", album: "Kick", year: 1987, genre: "New Wave" },
  { artist: "Blondie", album: "The Best of Blondie", year: 1981, genre: "New Wave" },
  { artist: "The Fixx", album: "Reach the Beach", year: 1983, genre: "New Wave" },
  { artist: "Naked Eyes", album: "Naked Eyes", year: 1983, genre: "Synth-Pop" },
  { artist: "Wang Chung", album: "Points on the Curve", year: 1983, genre: "New Wave" },
  { artist: "Simple Minds", album: "New Gold Dream", year: 1982, genre: "New Wave" },
  { artist: "Simple Minds", album: "Sparkle in the Rain", year: 1984, genre: "New Wave" },
  { artist: "The B-52's", album: "The B-52's", year: 1979, genre: "New Wave" },
  { artist: "The Boomtown Rats", album: "The Fine Art of Surfacing", year: 1979, genre: "New Wave" },
  { artist: "Ian Dury", album: "New Boots and Panties", year: 1977, genre: "New Wave" },
  { artist: "The The", album: "Soul Mining", year: 1983, genre: "New Wave" },
  { artist: "The Monroes", album: "The Monroes", year: 1982, genre: "New Wave" },
  { artist: "Big Country", album: "The Crossing", year: 1983, genre: "New Wave" },
  { artist: "Big Country", album: "Steeltown", year: 1984, genre: "New Wave" },
  { artist: "The Alarm", album: "Declaration", year: 1984, genre: "New Wave" },
  { artist: "Cyndi Lauper", album: "She's So Unusual", year: 1983, genre: "Pop" },
  { artist: "Elton John", album: "Goodbye Yellow Brick Road", year: 1973, genre: "Pop Rock" },
  { artist: "Elton John", album: "Captain Fantastic and the Brown Dirt Cowboy", year: 1975, genre: "Pop Rock" },
  { artist: "Elton John", album: "Empty Sky", year: 1969, genre: "Pop Rock" },
  { artist: "Elton John", album: "Elton John", year: 1970, genre: "Pop Rock" },
  { artist: "Elton John", album: "Honky Chateau", year: 1972, genre: "Pop Rock" },
  { artist: "Elton John", album: "Don't Shoot Me I'm Only the Piano Player", year: 1973, genre: "Pop Rock" },
  { artist: "Elton John", album: "Caribou", year: 1974, genre: "Pop Rock" },
  { artist: "Elton John", album: "Rock of the Westies", year: 1975, genre: "Pop Rock" },
  { artist: "Elton John", album: "11-17-70", year: 1971, genre: "Pop Rock" },
  { artist: "Elton John", album: "Jump Up", year: 1982, genre: "Pop Rock" },
  { artist: "Elton John", album: "Too Low for Zero", year: 1983, genre: "Pop Rock" },
  { artist: "Elton John", album: "Breaking Hearts", year: 1984, genre: "Pop Rock" },
  { artist: "Elton John", album: "Reg Strikes Back", year: 1988, genre: "Pop Rock" },
  { artist: "Elton John", album: "Greatest Hits", year: 1974, genre: "Pop Rock" },
  { artist: "Billy Joel", album: "The Stranger", year: 1977, genre: "Pop Rock" },
  { artist: "Billy Joel", album: "52nd Street", year: 1978, genre: "Pop Rock" },
  { artist: "Billy Joel", album: "Piano Man", year: 1973, genre: "Pop Rock" },
  { artist: "Billy Joel", album: "Turnstiles", year: 1976, genre: "Pop Rock" },
  { artist: "Billy Joel", album: "Glass Houses", year: 1980, genre: "Pop Rock" },
  { artist: "Billy Joel", album: "An Innocent Man", year: 1983, genre: "Pop Rock" },
  { artist: "Billy Joel", album: "Greatest Hits Volume I and II", year: 1985, genre: "Pop Rock" },
  { artist: "Michael Jackson", album: "Thriller", year: 1982, genre: "Pop" },
  { artist: "Michael Jackson", album: "Off the Wall", year: 1979, genre: "Pop" },
  { artist: "Prince", album: "Purple Rain", year: 1984, genre: "Pop" },
  { artist: "The Beatles", album: "Abbey Road", year: 1969, genre: "Rock" },
  { artist: "The Beatles", album: "1962-1966", year: 1973, genre: "Rock" },
  { artist: "The Beatles", album: "1967-1970", year: 1973, genre: "Rock" },
  { artist: "The Who", album: "Who's Next", year: 1971, genre: "Rock" },
  { artist: "The Who", album: "Greatest Hits", year: 1983, genre: "Rock" },
  { artist: "Whitney Houston", album: "Whitney", year: 1987, genre: "Pop" },
  { artist: "Sade", album: "Diamond Life", year: 1984, genre: "Soul" },
  { artist: "Terence Trent D'Arby", album: "Introducing the Hardline", year: 1987, genre: "R&B" },
  { artist: "Tracy Chapman", album: "Tracy Chapman", year: 1988, genre: "Folk Rock" },
  { artist: "Melissa Etheridge", album: "Melissa Etheridge", year: 1988, genre: "Rock" },
  { artist: "Christopher Cross", album: "Christopher Cross", year: 1979, genre: "Soft Rock" },
  { artist: "Rupert Holmes", album: "Partners in Crime", year: 1979, genre: "Soft Rock" },
  { artist: "The Rolling Stones", album: "Hot Rocks 1964-1971", year: 1971, genre: "Rock" },
  { artist: "The Rolling Stones", album: "Some Girls", year: 1978, genre: "Rock" },
  { artist: "The Rolling Stones", album: "Tattoo You", year: 1981, genre: "Rock" },
  { artist: "The Rolling Stones", album: "Emotional Rescue", year: 1980, genre: "Rock" },
  { artist: "The Rolling Stones", album: "Black and Blue", year: 1976, genre: "Rock" },
  { artist: "The Rolling Stones", album: "It's Only Rock n Roll", year: 1974, genre: "Rock" },
  { artist: "Rod Stewart", album: "The Rod Stewart Album", year: 1969, genre: "Rock" },
  { artist: "Rod Stewart", album: "A Night on the Town", year: 1976, genre: "Rock" },
  { artist: "Rod Stewart", album: "Foot Loose and Fancy Free", year: 1977, genre: "Rock" },
  { artist: "Rod Stewart", album: "Blondes Have More Fun", year: 1978, genre: "Rock" },
  { artist: "Rod Stewart", album: "Foolish Behaviour", year: 1980, genre: "Rock" },
  { artist: "Rod Stewart", album: "Tonight I'm Yours", year: 1981, genre: "Rock" },
  { artist: "Journey", album: "Escape", year: 1981, genre: "Arena Rock" },
  { artist: "Journey", album: "Frontiers", year: 1983, genre: "Arena Rock" },
  { artist: "Journey", album: "Departure", year: 1980, genre: "Arena Rock" },
  { artist: "Journey", album: "Journey", year: 1975, genre: "Arena Rock" },
  { artist: "Foreigner", album: "4", year: 1981, genre: "Arena Rock" },
  { artist: "Foreigner", album: "Double Vision", year: 1978, genre: "Arena Rock" },
  { artist: "Foreigner", album: "Head Games", year: 1979, genre: "Arena Rock" },
  { artist: "Foreigner", album: "Foreigner", year: 1977, genre: "Arena Rock" },
  { artist: "REO Speedwagon", album: "Hi Infidelity", year: 1980, genre: "Arena Rock" },
  { artist: "Toto", album: "Toto IV", year: 1982, genre: "Arena Rock" },
  { artist: "Toto", album: "Toto", year: 1978, genre: "Arena Rock" },
  { artist: "Asia", album: "Asia", year: 1982, genre: "Arena Rock" },
  { artist: "Heart", album: "Heart", year: 1985, genre: "Rock" },
  { artist: "Pat Benatar", album: "Crimes of Passion", year: 1980, genre: "Rock" },
  { artist: "Pat Benatar", album: "In the Heat of the Night", year: 1979, genre: "Rock" },
  { artist: "Pat Benatar", album: "Precious Time", year: 1981, genre: "Rock" },
  { artist: "Pat Benatar", album: "Get Nervous", year: 1982, genre: "Rock" },
  { artist: "Pat Benatar", album: "Tropico", year: 1984, genre: "Rock" },
  { artist: "Huey Lewis and the News", album: "Sports", year: 1983, genre: "Pop Rock" },
  { artist: "Huey Lewis and the News", album: "Fore!", year: 1986, genre: "Pop Rock" },
  { artist: "Huey Lewis and the News", album: "Small World", year: 1988, genre: "Pop Rock" },
  { artist: "Don Henley", album: "Building the Perfect Beast", year: 1984, genre: "Rock" },
  { artist: "Rick Springfield", album: "Working Class Dog", year: 1981, genre: "Pop Rock" },
  { artist: "Hall and Oates", album: "Voices", year: 1980, genre: "Pop Rock" },
  { artist: "Chicago", album: "Chicago IX Greatest Hits", year: 1975, genre: "Rock" },
  { artist: "Cheap Trick", album: "At Budokan", year: 1979, genre: "Rock" },
  { artist: "Cheap Trick", album: "Dream Police", year: 1979, genre: "Rock" },
  { artist: "Cheap Trick", album: "Lap of Luxury", year: 1988, genre: "Rock" },
  { artist: "Queen", album: "Greatest Hits", year: 1981, genre: "Rock" },
  { artist: "Queen", album: "The Game", year: 1980, genre: "Rock" },
  { artist: "Steve Perry", album: "Street Talk", year: 1984, genre: "Pop Rock" },
  { artist: "Steve Winwood", album: "Arc of a Diver", year: 1980, genre: "Pop Rock" },
  { artist: "Steve Winwood", album: "Roll With It", year: 1988, genre: "Pop Rock" },
  { artist: "Sting", album: "The Dream of the Blue Turtles", year: 1985, genre: "Rock" },
  { artist: "John Lennon", album: "Double Fantasy", year: 1980, genre: "Rock" },
  { artist: "Wings", album: "Wings Over America", year: 1976, genre: "Rock" },
  { artist: "Bryan Adams", album: "Reckless", year: 1984, genre: "Canadian" },
  { artist: "Bryan Adams", album: "Cuts Like a Knife", year: 1983, genre: "Canadian" },
  { artist: "Bryan Adams", album: "You Want It You Got It", year: 1981, genre: "Canadian" },
  { artist: "Bryan Adams", album: "Bryan Adams", year: 1980, genre: "Canadian" },
  { artist: "Loverboy", album: "Get Lucky", year: 1981, genre: "Canadian" },
  { artist: "Loverboy", album: "Keep It Up", year: 1983, genre: "Canadian" },
  { artist: "Loverboy", album: "Loverboy", year: 1980, genre: "Canadian" },
  { artist: "April Wine", album: "The Nature of the Beast", year: 1981, genre: "Canadian" },
  { artist: "April Wine", album: "Power Play", year: 1982, genre: "Canadian" },
  { artist: "April Wine", album: "Harder Faster", year: 1979, genre: "Canadian" },
  { artist: "April Wine", album: "Greatest Hits", year: 1979, genre: "Canadian" },
  { artist: "Corey Hart", album: "Boy in the Box", year: 1985, genre: "Canadian" },
  { artist: "Corey Hart", album: "First Offense", year: 1983, genre: "Canadian" },
  { artist: "Glass Tiger", album: "The Thin Red Line", year: 1986, genre: "Canadian" },
  { artist: "Platinum Blonde", album: "Standing in the Dark", year: 1983, genre: "Canadian" },
  { artist: "Trooper", album: "Hot Shots", year: 1979, genre: "Canadian" },
  { artist: "Chilliwack", album: "Segue", year: 1983, genre: "Canadian" },
  { artist: "Prism", album: "The Best of Prism", year: 1980, genre: "Canadian" },
  { artist: "Bachman-Turner Overdrive", album: "Not Fragile", year: 1974, genre: "Canadian" },
  { artist: "Bachman-Turner Overdrive", album: "Four Wheel Drive", year: 1975, genre: "Canadian" },
  { artist: "Burton Cummings", album: "Dream of a Child", year: 1978, genre: "Canadian" },
  { artist: "Headpins", album: "Line of Fire", year: 1983, genre: "Canadian" },
  { artist: "Harlequin", album: "Love Crimes", year: 1980, genre: "Canadian" },
  { artist: "Toronto", album: "Girls Night Out", year: 1983, genre: "Canadian" },
  { artist: "Toronto", album: "Greatest Hits", year: 1984, genre: "Canadian" },
  { artist: "Streetheart", album: "Streetheart", year: 1982, genre: "Canadian" },
  { artist: "Stonebolt", album: "Stonebolt", year: 1978, genre: "Canadian" },
  { artist: "Payolas", album: "Hammer on a Drum", year: 1983, genre: "Canadian" },
  { artist: "Spoons", album: "Talkback", year: 1983, genre: "Canadian" },
  { artist: "Boys Brigade", album: "Boys Brigade", year: 1983, genre: "Canadian" },
  { artist: "Rough Trade", album: "Weapons", year: 1983, genre: "Canadian" },
  { artist: "Luba", album: "Secrets and Sins", year: 1984, genre: "Canadian" },
  { artist: "Powder Blues Band", album: "Uncut", year: 1980, genre: "Canadian" },
  { artist: "Powder Blues Band", album: "Thirsty Ears", year: 1981, genre: "Canadian" },
  { artist: "Queen City Kids", album: "Black Box", year: 1982, genre: "Canadian" },
  { artist: "Doucette", album: "Mama Let Him Play", year: 1977, genre: "Canadian" },
  { artist: "Anne Murray", album: "A Little Good News", year: 1983, genre: "Canadian" },
  { artist: "Alannah Myles", album: "Alannah Myles", year: 1989, genre: "Canadian" },
  { artist: "Northern Lights", album: "Tears Are Not Enough", year: 1985, genre: "Canadian" },
  { artist: "Diane Dufresne", album: "Strip Tease", year: 1979, genre: "Canadian" },
  { artist: "Bob and Doug McKenzie", album: "Great White North", year: 1981, genre: "Comedy" },
  { artist: "Steve Martin", album: "Let's Get Small", year: 1977, genre: "Comedy" },
  { artist: "Steve Martin", album: "A Wild and Crazy Guy", year: 1978, genre: "Comedy" },
  { artist: "Various Artists", album: "Saturday Night Fever", year: 1977, genre: "Soundtrack" },
  { artist: "Various Artists", album: "Flashdance", year: 1983, genre: "Soundtrack" },
  { artist: "Various Artists", album: "Footloose", year: 1984, genre: "Soundtrack" },
  { artist: "Various Artists", album: "The Rocky Horror Picture Show", year: 1975, genre: "Soundtrack" },
  { artist: "Various Artists", album: "The Big Chill", year: 1983, genre: "Soundtrack" },
  { artist: "Various Artists", album: "More Songs from The Big Chill", year: 1984, genre: "Soundtrack" },
  { artist: "Various Artists", album: "Stand by Me", year: 1986, genre: "Soundtrack" },
  { artist: "Various Artists", album: "American Graffiti", year: 1973, genre: "Soundtrack" },
  { artist: "Various Artists", album: "Woodstock", year: 1970, genre: "Soundtrack" },
  { artist: "Various Artists", album: "Weird Science", year: 1985, genre: "Soundtrack" },
  { artist: "Various Artists", album: "Eddie and the Cruisers", year: 1983, genre: "Soundtrack" },
  { artist: "Various Artists", album: "Against All Odds", year: 1984, genre: "Soundtrack" },
  { artist: "B.B. King", album: "Live in Cook County Jail", year: 1971, genre: "Blues" },
  { artist: "B.B. King", album: "The Best of B.B. King", year: 1973, genre: "Blues" },
  { artist: "B.B. King", album: "The Incredible Soul of B.B. King", year: 1967, genre: "Blues" },
  { artist: "B.B. King", album: "Midnight Believer", year: 1978, genre: "Blues" },
  { artist: "Bob Marley", album: "Legend", year: 1984, genre: "Reggae" },
  { artist: "Peter Tosh", album: "Legalize It", year: 1976, genre: "Reggae" },
  { artist: "Peter Tosh", album: "Equal Rights", year: 1977, genre: "Reggae" },
  { artist: "UB40", album: "Labour of Love", year: 1983, genre: "Reggae" },
  { artist: "UB40", album: "Geffery Morgan", year: 1984, genre: "Reggae" },
  { artist: "Commodores", album: "Midnight Magic", year: 1979, genre: "Soul" },
  { artist: "Commodores", album: "Greatest Hits", year: 1978, genre: "Soul" },
  { artist: "The Honeydrippers", album: "Volume One", year: 1984, genre: "R&B" },
  { artist: "U2", album: "The Joshua Tree", year: 1987, genre: "Alternative" },
  { artist: "U2", album: "War", year: 1983, genre: "Alternative" },
  { artist: "U2", album: "The Unforgettable Fire", year: 1984, genre: "Alternative" },
  { artist: "U2", album: "Under a Blood Red Sky", year: 1983, genre: "Alternative" },
  { artist: "Midnight Oil", album: "Diesel and Dust", year: 1987, genre: "Alternative" },
  { artist: "Dire Straits", album: "Brothers in Arms", year: 1985, genre: "Rock" },
  { artist: "The Pretenders", album: "Learning to Crawl", year: 1984, genre: "Rock" },
  { artist: "Elvis Costello", album: "The Best of Elvis Costello", year: 1985, genre: "New Wave" },
  { artist: "Elvis Costello", album: "Punch the Clock", year: 1983, genre: "New Wave" },
  { artist: "Joe Jackson", album: "Look Sharp!", year: 1979, genre: "New Wave" },
  { artist: "Joe Jackson", album: "I'm the Man", year: 1979, genre: "New Wave" },
  { artist: "Joe Jackson", album: "Body and Soul", year: 1984, genre: "New Wave" },
  { artist: "The Kinks", album: "Give the People What They Want", year: 1981, genre: "Rock" },
  { artist: "The Kinks", album: "State of Confusion", year: 1983, genre: "Rock" },
  { artist: "The Kinks", album: "One for the Road", year: 1980, genre: "Rock" },
  { artist: "Los Lobos", album: "How Will the Wolf Survive", year: 1984, genre: "Rock" },
  { artist: "Bob Seger", album: "Night Moves", year: 1976, genre: "Heartland Rock" },
  { artist: "Bob Seger", album: "Against the Wind", year: 1980, genre: "Heartland Rock" },
  { artist: "Bob Seger", album: "Nine Tonight", year: 1981, genre: "Heartland Rock" },
  { artist: "John Cougar", album: "American Fool", year: 1982, genre: "Heartland Rock" },
  { artist: "John Cougar Mellencamp", album: "Uh-Huh", year: 1983, genre: "Heartland Rock" },
  { artist: "John Fogerty", album: "Centerfield", year: 1985, genre: "Heartland Rock" },
  { artist: "John Cafferty", album: "Tough All Over", year: 1985, genre: "Heartland Rock" },
  { artist: "Jackson Browne", album: "Running on Empty", year: 1977, genre: "Rock" },
  { artist: "Jackson Browne", album: "Hold Out", year: 1980, genre: "Rock" },
  { artist: "Lynyrd Skynyrd", album: "Gold and Platinum", year: 1979, genre: "Southern Rock" },
  { artist: "Steve Miller Band", album: "Fly Like an Eagle", year: 1976, genre: "Rock" },
  { artist: "Steve Miller Band", album: "Greatest Hits 1974-78", year: 1978, genre: "Rock" },
  { artist: "Air Supply", album: "Lost in Love", year: 1980, genre: "Soft Rock" },
  { artist: "Air Supply", album: "The One That You Love", year: 1981, genre: "Soft Rock" },
  { artist: "Al Stewart", album: "Year of the Cat", year: 1976, genre: "Soft Rock" },
  { artist: "Chris de Burgh", album: "Spanish Train and Other Stories", year: 1975, genre: "Soft Rock" },
  { artist: "Cliff Richard", album: "I'm No Hero", year: 1980, genre: "Pop" },
  { artist: "Dr. Hook", album: "Greatest Hits", year: 1980, genre: "Soft Rock" },
  { artist: "Dr. Hook", album: "Pleasure and Pain", year: 1978, genre: "Soft Rock" },
  { artist: "Juice Newton", album: "Juice", year: 1981, genre: "Country" },
  { artist: "Little River Band", album: "Sleeper Catcher", year: 1978, genre: "Soft Rock" },
  { artist: "Bee Gees", album: "Spirits Having Flown", year: 1979, genre: "Disco" },
  { artist: "Bee Gees", album: "Main Course", year: 1975, genre: "Disco" },
  { artist: "Village People", album: "Go West", year: 1979, genre: "Disco" },
  { artist: "Steely Dan", album: "Gold", year: 1982, genre: "Rock" },
  { artist: "Simon and Garfunkel", album: "Greatest Hits", year: 1972, genre: "Folk Rock" },
  { artist: "Jay Ferguson", album: "Thunder Island", year: 1978, genre: "Rock" },
  { artist: "Stray Cats", album: "Built for Speed", year: 1982, genre: "Rockabilly" },
  { artist: "Stray Cats", album: "Rant N Rave with the Stray Cats", year: 1983, genre: "Rockabilly" },
  { artist: "The Blasters", album: "The Blasters", year: 1981, genre: "Rockabilly" },
  { artist: "The Blasters", album: "Non-Fiction", year: 1983, genre: "Rockabilly" },
  { artist: "Chuck Berry", album: "Golden Hits", year: 1967, genre: "Rock and Roll" },
  { artist: "Jerry Lee Lewis", album: "Greatest Hits", year: 1982, genre: "Rock and Roll" },
  { artist: "Jan and Dean", album: "Greatest Hits", year: 1966, genre: "Surf Rock" },
  { artist: "Alabama", album: "Mountain Music", year: 1982, genre: "Country" },
  { artist: "Alabama", album: "The Closer You Get", year: 1983, genre: "Country" },
  { artist: "Merle Haggard", album: "Let Me Tell You About a Song", year: 1972, genre: "Country" },
  { artist: "Various Artists", album: "Reggae Greats", year: 1985, genre: "Compilation" },
  { artist: "Various Artists", album: "History of U.S. Rock", year: 1982, genre: "Compilation" },
  { artist: "Various Artists", album: "Stars on Long Play", year: 1981, genre: "Compilation" },
  { artist: "Various Artists", album: "The Secret Policeman's Other Ball", year: 1982, genre: "Compilation" },
  { artist: "Various Artists", album: "Pure Gold Collection", year: 1986, genre: "Compilation" },
  { artist: "Various Artists", album: "Get It On", year: 1985, genre: "Compilation" },
  { artist: "Band Aid", album: "Do They Know It's Christmas", year: 1984, genre: "Pop" },
];

const stringToColor = (s) => { let h = 0; for (let i = 0; i < s.length; i++) h = s.charCodeAt(i) + ((h << 5) - h); return `hsl(${Math.abs(h % 360)}, 70%, 35%)`; };

const AlbumCard = ({ album, idx, onSelect }) => {
  const [hover, setHover] = useState(false);
  const [imgLoaded, setImgLoaded] = useState(false);
  const [imgError, setImgError] = useState(false);
  const key = `${album.artist}-${album.album}`;
  const artUrl = artworkUrls[key];
  const fallback = `linear-gradient(135deg, ${stringToColor(album.artist)} 0%, ${stringToColor(album.album)} 100%)`;
  const spotifyUrl = `https://open.spotify.com/search/${encodeURIComponent(`${album.artist} ${album.album}`)}`;

  return (
    <div className="album-card group cursor-pointer animate-slideUp" style={{ animationDelay: `${Math.min(idx * 15, 400)}ms` }} onClick={() => onSelect(album, artUrl)} onMouseEnter={() => setHover(true)} onMouseLeave={() => setHover(false)}>
      <div className="relative aspect-square rounded-xl overflow-hidden shadow-2xl bg-zinc-900">
        <div className="album-art absolute inset-0 transition-transform duration-500">
          {artUrl && !imgError ? (
            <>
              {!imgLoaded && <div className="absolute inset-0 bg-zinc-800 animate-pulse" />}
              <img src={artUrl} alt={album.album} className={`w-full h-full object-cover transition-opacity duration-300 ${imgLoaded ? 'opacity-100' : 'opacity-0'}`} onLoad={() => setImgLoaded(true)} onError={() => setImgError(true)} loading="lazy" />
            </>
          ) : (
            <div className="absolute inset-0" style={{ background: fallback }}>
              <div className="absolute inset-0 flex items-center justify-center p-4 text-center">
                <div><p className="text-white font-bold text-sm drop-shadow-lg">{album.album}</p><p className="text-white/70 text-xs mt-1">{album.artist}</p></div>
              </div>
            </div>
          )}
        </div>
        <div className="vinyl-disc absolute right-0 top-1/2 -translate-y-1/2 w-[85%] h-[85%] rounded-full transition-transform duration-500 translate-x-full z-0">
          <div className="absolute inset-0 rounded-full bg-zinc-950 shadow-xl">
            <div className="absolute inset-[3%] rounded-full bg-zinc-900" />
            <div className="absolute inset-[6%] rounded-full bg-zinc-950" />
            <div className="absolute inset-[15%] rounded-full border border-zinc-800" />
            <div className="absolute inset-[25%] rounded-full border border-zinc-800" />
            <div className="absolute inset-[35%] rounded-full bg-gradient-to-br from-red-600 to-red-800" />
            <div className="absolute inset-[42%] rounded-full bg-zinc-950" />
          </div>
        </div>
        <div className="absolute inset-0 bg-gradient-to-t from-black/90 via-black/20 to-transparent opacity-0 group-hover:opacity-100 transition-opacity duration-300 z-10" />
        <div className={`absolute bottom-4 right-4 z-20 transition-all duration-300 ${hover ? 'opacity-100 scale-100' : 'opacity-0 scale-75'}`}>
          <a href={spotifyUrl} target="_blank" rel="noopener noreferrer" onClick={(e) => e.stopPropagation()} className="flex w-12 h-12 rounded-full bg-green-500 hover:bg-green-400 hover:scale-110 transition-all shadow-lg items-center justify-center">
            <svg className="w-5 h-5 text-black ml-0.5" fill="currentColor" viewBox="0 0 24 24"><path d="M8 5v14l11-7z"/></svg>
          </a>
        </div>
        <div className={`absolute top-3 left-3 z-20 transition-all duration-300 ${hover ? 'opacity-100' : 'opacity-0'}`}>
          <span className="px-2 py-1 bg-black/70 backdrop-blur-sm rounded-full text-xs font-medium text-white">{album.year}</span>
        </div>
      </div>
      <div className="mt-3 px-1">
        <h3 className="text-white font-semibold text-sm truncate group-hover:text-green-400 transition-colors">{album.album}</h3>
        <p className="text-zinc-500 text-xs truncate mt-0.5">{album.artist}</p>
        <p className="text-zinc-600 text-xs mt-1">{album.genre}</p>
      </div>
    </div>
  );
};

const Modal = ({ album, artUrl, onClose }) => {
  if (!album) return null;
  const spotifyUrl = `https://open.spotify.com/search/${encodeURIComponent(`${album.artist} ${album.album}`)}`;
  const fallback = `linear-gradient(135deg, ${stringToColor(album.artist)} 0%, ${stringToColor(album.album)} 100%)`;
  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/90 backdrop-blur-xl animate-fadeIn" onClick={onClose}>
      <div className="relative bg-zinc-900 rounded-3xl max-w-2xl w-full overflow-hidden shadow-2xl border border-zinc-800" onClick={e => e.stopPropagation()}>
        <div className="absolute inset-0 overflow-hidden">
          {artUrl ? <img src={artUrl} alt="" className="w-full h-full object-cover scale-150 blur-3xl opacity-30" /> : <div className="w-full h-full" style={{ background: fallback, opacity: 0.3 }} />}
        </div>
        <div className="relative p-6 md:p-8">
          <button onClick={onClose} className="absolute top-4 right-4 w-10 h-10 rounded-full bg-zinc-800/80 hover:bg-zinc-700 flex items-center justify-center"><svg className="w-5 h-5 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" /></svg></button>
          <div className="flex flex-col md:flex-row gap-6">
            <div className="w-full md:w-64 flex-shrink-0">
              <div className="aspect-square rounded-2xl overflow-hidden shadow-2xl">
                {artUrl ? <img src={artUrl} alt={album.album} className="w-full h-full object-cover" /> : <div className="w-full h-full flex items-center justify-center" style={{ background: fallback }}><div className="text-center p-4"><p className="text-white text-xl font-bold">{album.album}</p><p className="text-white/70 mt-2">{album.artist}</p></div></div>}
              </div>
            </div>
            <div className="flex-1 flex flex-col justify-center">
              <span className="text-green-500 text-sm font-medium uppercase tracking-wider">{album.genre}</span>
              <h2 className="text-3xl md:text-4xl font-bold text-white mt-2 leading-tight">{album.album}</h2>
              <p className="text-xl text-zinc-400 mt-2">{album.artist}</p>
              <p className="text-zinc-500 mt-1">{album.year}</p>
              <div className="flex gap-3 mt-6">
                <a href={spotifyUrl} target="_blank" rel="noopener noreferrer" className="flex items-center gap-2 px-6 py-3 bg-green-500 hover:bg-green-400 text-black font-semibold rounded-full transition-all hover:scale-105">
                  <svg className="w-5 h-5" viewBox="0 0 24 24" fill="currentColor"><path d="M12 0C5.4 0 0 5.4 0 12s5.4 12 12 12 12-5.4 12-12S18.66 0 12 0zm5.521 17.34c-.24.359-.66.48-1.021.24-2.82-1.74-6.36-2.101-10.561-1.141-.418.122-.779-.179-.899-.539-.12-.421.18-.78.54-.9 4.56-1.021 8.52-.6 11.64 1.32.42.18.479.659.301 1.02zm1.44-3.3c-.301.42-.841.6-1.262.3-3.239-1.98-8.159-2.58-11.939-1.38-.479.12-1.02-.12-1.14-.6-.12-.48.12-1.021.6-1.141C9.6 9.9 15 10.561 18.72 12.84c.361.181.54.78.241 1.2zm.12-3.36C15.24 8.4 8.82 8.16 5.16 9.301c-.6.179-1.2-.181-1.38-.721-.18-.601.18-1.2.72-1.381 4.26-1.26 11.28-1.02 15.721 1.621.539.3.719 1.02.419 1.56-.299.421-1.02.599-1.559.3z"/></svg>
                  Play on Spotify
                </a>
              </div>
            </div>
          </div>
        </div>
      </div>
    </div>
  );
};

function App() {
  const [search, setSearch] = useState("");
  const [genre, setGenre] = useState("All");
  const [decade, setDecade] = useState("All");
  const [sort, setSort] = useState("artist");
  const [selectedAlbum, setSelectedAlbum] = useState(null);
  const [selectedArt, setSelectedArt] = useState(null);

  const genres = useMemo(() => ["All", ...new Set(vinylData.map(a => a.genre))].sort(), []);

  const filtered = useMemo(() => {
    let r = vinylData;
    if (search) { const t = search.toLowerCase(); r = r.filter(a => a.artist.toLowerCase().includes(t) || a.album.toLowerCase().includes(t)); }
    if (genre !== "All") r = r.filter(a => a.genre === genre);
    if (decade !== "All") { const s = parseInt(decade); r = r.filter(a => a.year >= s && a.year < s + 10); }
    return [...r].sort((a, b) => sort === "artist" ? a.artist.localeCompare(b.artist) : sort === "album" ? a.album.localeCompare(b.album) : b.year - a.year);
  }, [search, genre, decade, sort]);

  const stats = { total: vinylData.length, artists: new Set(vinylData.map(a => a.artist)).size, genres: new Set(vinylData.map(a => a.genre)).size, covers: Object.keys(artworkUrls).length };

  return (
    <div className="min-h-screen bg-black text-white">
      <div className="fixed inset-0 overflow-hidden pointer-events-none">
        <div className="absolute inset-0 bg-gradient-to-br from-green-950/30 via-black to-purple-950/20" />
      </div>
      <header className="sticky top-0 z-40 bg-black/80 backdrop-blur-2xl border-b border-zinc-800/50">
        <div className="max-w-[1800px] mx-auto px-4 md:px-6 py-4">
          <div className="flex items-center justify-between gap-4 flex-wrap">
            <div className="flex items-center gap-4">
              <div className="w-12 h-12 rounded-full bg-gradient-to-br from-green-400 to-green-600 flex items-center justify-center shadow-lg shadow-green-500/20">
                <svg className="w-6 h-6 text-black" viewBox="0 0 24 24" fill="currentColor"><path d="M12 0C5.4 0 0 5.4 0 12s5.4 12 12 12 12-5.4 12-12S18.66 0 12 0zm5.521 17.34c-.24.359-.66.48-1.021.24-2.82-1.74-6.36-2.101-10.561-1.141-.418.122-.779-.179-.899-.539-.12-.421.18-.78.54-.9 4.56-1.021 8.52-.6 11.64 1.32.42.18.479.659.301 1.02zm1.44-3.3c-.301.42-.841.6-1.262.3-3.239-1.98-8.159-2.58-11.939-1.38-.479.12-1.02-.12-1.14-.6-.12-.48.12-1.021.6-1.141C9.6 9.9 15 10.561 18.72 12.84c.361.181.54.78.241 1.2zm.12-3.36C15.24 8.4 8.82 8.16 5.16 9.301c-.6.179-1.2-.181-1.38-.721-.18-.601.18-1.2.72-1.381 4.26-1.26 11.28-1.02 15.721 1.621.539.3.719 1.02.419 1.56-.299.421-1.02.599-1.559.3z"/></svg>
              </div>
              <div>
                <h1 className="text-xl font-bold text-white">Vinyl Collection</h1>
                <p className="text-zinc-500 text-sm">{stats.total} albums • {stats.covers} covers</p>
              </div>
            </div>
            <div className="flex-1 max-w-xl">
              <div className="relative">
                <input type="text" placeholder="Search artists or albums..." value={search} onChange={(e) => setSearch(e.target.value)} className="w-full bg-zinc-900/80 border border-zinc-800 rounded-full px-5 py-3 pl-12 text-sm focus:outline-none focus:border-green-500/50 focus:ring-2 focus:ring-green-500/20 transition-all placeholder:text-zinc-600" />
                <svg className="absolute left-4 top-1/2 -translate-y-1/2 w-5 h-5 text-zinc-500" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M21 21l-6-6m2-5a7 7 0 11-14 0 7 7 0 0114 0z" /></svg>
              </div>
            </div>
          </div>
        </div>
      </header>
      <main className="relative z-10 max-w-[1800px] mx-auto px-4 md:px-6 py-6">
        <div className="grid grid-cols-2 md:grid-cols-4 gap-3 mb-6">
          {[{ l: "Albums", v: stats.total }, { l: "Artists", v: stats.artists }, { l: "Genres", v: stats.genres }, { l: "Covers", v: stats.covers }].map(s => (
            <div key={s.l} className="bg-zinc-900/50 backdrop-blur border border-zinc-800/50 rounded-2xl p-4">
              <p className="text-3xl font-bold text-green-500">{s.v}</p>
              <p className="text-zinc-500 text-sm">{s.l}</p>
            </div>
          ))}
        </div>
        <div className="flex flex-wrap items-center gap-3 mb-8">
          <select value={genre} onChange={(e) => setGenre(e.target.value)} className="bg-zinc-900 border border-zinc-800 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:border-green-500 cursor-pointer">
            {genres.map(g => <option key={g} value={g}>{g === "All" ? "All Genres" : g}</option>)}
          </select>
          <select value={decade} onChange={(e) => setDecade(e.target.value)} className="bg-zinc-900 border border-zinc-800 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:border-green-500 cursor-pointer">
            <option value="All">All Decades</option><option value="1960">1960s</option><option value="1970">1970s</option><option value="1980">1980s</option>
          </select>
          <select value={sort} onChange={(e) => setSort(e.target.value)} className="bg-zinc-900 border border-zinc-800 rounded-xl px-4 py-2.5 text-sm focus:outline-none focus:border-green-500 cursor-pointer">
            <option value="artist">Sort: Artist</option><option value="album">Sort: Album</option><option value="year">Sort: Year</option>
          </select>
          <div className="flex-1" /><p className="text-zinc-500 text-sm">{filtered.length} albums</p>
        </div>
        <div className="grid grid-cols-2 sm:grid-cols-3 md:grid-cols-4 lg:grid-cols-5 xl:grid-cols-6 2xl:grid-cols-7 gap-5">
          {filtered.map((a, i) => <AlbumCard key={`${a.artist}-${a.album}`} album={a} idx={i} onSelect={(alb, art) => { setSelectedAlbum(alb); setSelectedArt(art); }} />)}
        </div>
        {filtered.length === 0 && <div className="text-center py-20"><p className="text-zinc-500 text-lg">No albums found</p></div>}
      </main>
      {selectedAlbum && <Modal album={selectedAlbum} artUrl={selectedArt} onClose={() => setSelectedAlbum(null)} />}
    </div>
  );
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```

  </script>
</body>
</html>