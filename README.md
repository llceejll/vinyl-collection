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
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700;800&display=swap" rel="stylesheet">
  <style>
    * { font-family: 'Inter', sans-serif; }
    @keyframes shimmer { 0% { background-position: -200% 0; } 100% { background-position: 200% 0; } }
    @keyframes fadeIn { from { opacity: 0; transform: scale(0.95); } to { opacity: 1; transform: scale(1); } }
    @keyframes slideUp { from { opacity: 0; transform: translateY(20px); } to { opacity: 1; transform: translateY(0); } }
    .animate-shimmer { background: linear-gradient(90deg, #27272a 25%, #3f3f46 50%, #27272a 75%); background-size: 200% 100%; animation: shimmer 1.5s infinite; }
    .animate-fadeIn { animation: fadeIn 0.3s ease-out forwards; }
    .animate-slideUp { animation: slideUp 0.4s ease-out forwards; }
    .album-card:hover .vinyl-disc { transform: translateX(30%); }
    .album-card:hover .album-art { transform: scale(1.05); }
  </style>
</head>
<body class="bg-black">
  <div id="root"></div>
  <script type="text/babel">
    const { useState, useEffect, useMemo } = React;

```
const vinylData = [
  { artist: "AC/DC", album: "Back in Black", year: 1980, genre: "Hard Rock" },
  { artist: "AC/DC", album: "Highway to Hell", year: 1979, genre: "Hard Rock" },
  { artist: "AC/DC", album: "For Those About to Rock We Salute You", year: 1981, genre: "Hard Rock" },
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
  { artist: "Steve Martin", album: "Let's Get Small", year: 1977, genre: "Comedy" },
  { artist: "Steve Martin", album: "A Wild and Crazy Guy", year: 1978, genre: "Comedy" },
  { artist: "Various Artists", album: "Reggae Greats", year: 1985, genre: "Compilation" },
  { artist: "Various Artists", album: "History of U.S. Rock", year: 1982, genre: "Compilation" },
  { artist: "Various Artists", album: "Stars on Long Play", year: 1981, genre: "Compilation" },
  { artist: "Various Artists", album: "The Secret Policeman's Other Ball", year: 1982, genre: "Compilation" },
  { artist: "Various Artists", album: "Pure Gold Collection", year: 1986, genre: "Compilation" },
  { artist: "Various Artists", album: "Get It On", year: 1985, genre: "Compilation" },
  { artist: "Band Aid", album: "Do They Know It's Christmas", year: 1984, genre: "Pop" },
];

const artworkCache = {};
const fetchArtwork = async (artist, album) => {
  const key = `${artist}-${album}`;
  if (artworkCache[key]) return artworkCache[key];
  try {
    const q = encodeURIComponent(`${artist} ${album}`);
    const r = await fetch(`https://itunes.apple.com/search?term=${q}&media=music&entity=album&limit=1`);
    const d = await r.json();
    if (d.results?.[0]) {
      const art = d.results[0].artworkUrl100.replace('100x100', '600x600');
      artworkCache[key] = art;
      return art;
    }
  } catch (e) {}
  return null;
};

const stringToColor = (s) => {
  let h = 0;
  for (let i = 0; i < s.length; i++) h = s.charCodeAt(i) + ((h << 5) - h);
  return `hsl(${Math.abs(h % 360)}, 70%, 35%)`;
};

const LazyImage = ({ src, fallback, alt, className }) => {
  const [loaded, setLoaded] = useState(false);
  const [error, setError] = useState(false);
  if (error || !src) return <div className={className} style={{ background: fallback }} />;
  return (
    <>
      {!loaded && <div className={`${className} animate-shimmer`} />}
      <img src={src} alt={alt} className={`${className} ${loaded ? 'opacity-100' : 'opacity-0'} transition-opacity duration-300`} onLoad={() => setLoaded(true)} onError={() => setError(true)} loading="lazy" />
    </>
  );
};

const AlbumCard = ({ album, artworkUrl, onSelect, idx }) => {
  const [hover, setHover] = useState(false);
  const fallback = `linear-gradient(135deg, ${stringToColor(album.artist)} 0%, ${stringToColor(album.album)} 100%)`;
  const spotifyUrl = `https://open.spotify.com/search/${encodeURIComponent(`${album.artist} ${album.album}`)}`;

  return (
    <div className="album-card group cursor-pointer animate-slideUp" style={{ animationDelay: `${Math.min(idx * 15, 400)}ms` }} onClick={() => onSelect(album, artworkUrl)} onMouseEnter={() => setHover(true)} onMouseLeave={() => setHover(false)}>
      <div className="relative aspect-square rounded-xl overflow-hidden shadow-2xl bg-zinc-900">
        <div className="album-art absolute inset-0 transition-transform duration-500">
          <LazyImage src={artworkUrl} fallback={fallback} alt={album.album} className="absolute inset-0 w-full h-full object-cover" />
        </div>
        <div className="vinyl-disc absolute right-0 top-1/2 -translate-y-1/2 w-[85%] h-[85%] rounded-full transition-transform duration-500 translate-x-full z-0">
          <div className="absolute inset-0 rounded-full bg-zinc-950 shadow-xl">
            <div className="absolute inset-[3%] rounded-full bg-zinc-900" />
            <div className="absolute inset-[6%] rounded-full bg-zinc-950" />
            <div className="absolute inset-[15%] rounded-full border border-zinc-800" />
            <div className="absolute inset-[25%] rounded-full border border-zinc-800" />
            <div className="absolute inset-[35%] rounded-full bg-gradient-to-br from-red-600 to-red-800" />
            <div className="absolute inset-[42%] rounded-full bg-zinc-950" />
            <div className="absolute inset-0 rounded-full bg-gradient-to-br from-white/10 via-transparent to-transparent" />
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

const Modal = ({ album, artworkUrl, onClose }) => {
  if (!album) return null;
  const spotifyUrl = `https://open.spotify.com/search/${encodeURIComponent(`${album.artist} ${album.album}`)}`;
  const fallback = `linear-gradient(135deg, ${stringToColor(album.artist)} 0%, ${stringToColor(album.album)} 100%)`;
  return (
    <div className="fixed inset-0 z-50 flex items-center justify-center p-4 bg-black/90 backdrop-blur-xl animate-fadeIn" onClick={onClose}>
      <div className="relative bg-zinc-900 rounded-3xl max-w-2xl w-full overflow-hidden shadow-2xl border border-zinc-800" onClick={e => e.stopPropagation()}>
        <div className="absolute inset-0 overflow-hidden"><LazyImage src={artworkUrl} fallback={fallback} alt="" className="w-full h-full object-cover scale-150 blur-3xl opacity-30" /></div>
        <div className="relative p-6 md:p-8">
          <button onClick={onClose} className="absolute top-4 right-4 w-10 h-10 rounded-full bg-zinc-800/80 hover:bg-zinc-700 flex items-center justify-center"><svg className="w-5 h-5 text-white" fill="none" stroke="currentColor" viewBox="0 0 24 24"><path strokeLinecap="round" strokeLinejoin="round" strokeWidth={2} d="M6 18L18 6M6 6l12 12" /></svg></button>
          <div className="flex flex-col md:flex-row gap-6">
            <div className="w-full md:w-64 flex-shrink-0"><div className="aspect-square rounded-2xl overflow-hidden shadow-2xl"><LazyImage src={artworkUrl} fallback={fallback} alt={album.album} className="w-full h-full object-cover" /></div></div>
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
  const [artworks, setArtworks] = useState({});
  const [loadedCount, setLoadedCount] = useState(0);

  const genres = useMemo(() => ["All", ...new Set(vinylData.map(a => a.genre))].sort(), []);

  useEffect(() => {
    let mounted = true;
    const load = async () => {
      for (let i = 0; i < vinylData.length; i += 10) {
        const batch = vinylData.slice(i, i + 10);
        await Promise.all(batch.map(async (a) => {
          const k = `${a.artist}-${a.album}`;
          const art = await fetchArtwork(a.artist, a.album);
          if (art && mounted) {
            setArtworks(p => ({ ...p, [k]: art }));
            setLoadedCount(c => c + 1);
          }
        }));
        await new Promise(r => setTimeout(r, 50));
      }
    };
    load();
    return () => { mounted = false; };
  }, []);

  const filtered = useMemo(() => {
    let r = vinylData;
    if (search) { const t = search.toLowerCase(); r = r.filter(a => a.artist.toLowerCase().includes(t) || a.album.toLowerCase().includes(t)); }
    if (genre !== "All") r = r.filter(a => a.genre === genre);
    if (decade !== "All") { const s = parseInt(decade); r = r.filter(a => a.year >= s && a.year < s + 10); }
    return [...r].sort((a, b) => sort === "artist" ? a.artist.localeCompare(b.artist) : sort === "album" ? a.album.localeCompare(b.album) : b.year - a.year);
  }, [search, genre, decade, sort]);

  const stats = { total: vinylData.length, artists: new Set(vinylData.map(a => a.artist)).size, genres: new Set(vinylData.map(a => a.genre)).size, art: Object.keys(artworks).length };

  return (
    <div className="min-h-screen bg-black text-white">
      <div className="fixed inset-0 overflow-hidden pointer-events-none">
        <div className="absolute inset-0 bg-gradient-to-br from-green-950/30 via-black to-purple-950/20" />
        <div className="absolute top-0 left-1/4 w-[500px] h-[500px] bg-green-500/5 rounded-full blur-3xl" />
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
                <div className="flex items-center gap-2 text-sm">
                  <span className="text-zinc-500">{stats.total} albums</span>
                  {loadedCount < stats.total && <span className="text-green-500 flex items-center gap-1"><span className="w-1.5 h-1.5 rounded-full bg-green-500 animate-pulse" />Loading artwork {loadedCount}/{stats.total}</span>}
                  {loadedCount >= stats.total && <span className="text-green-500">✓ {stats.art} covers loaded</span>}
                </div>
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
          {[{ l: "Albums", v: stats.total }, { l: "Artists", v: stats.artists }, { l: "Genres", v: stats.genres }, { l: "Covers", v: stats.art }].map(s => (
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
          {filtered.map((a, i) => <AlbumCard key={`${a.artist}-${a.album}`} album={a} idx={i} artworkUrl={artworks[`${a.artist}-${a.album}`]} onSelect={(alb, art) => { setSelectedAlbum(alb); setSelectedArt(art); }} />)}
        </div>
        {filtered.length === 0 && <div className="text-center py-20"><p className="text-zinc-500 text-lg">No albums found</p></div>}
      </main>
      {selectedAlbum && <Modal album={selectedAlbum} artworkUrl={selectedArt} onClose={() => setSelectedAlbum(null)} />}
    </div>
  );
}

ReactDOM.createRoot(document.getElementById('root')).render(<App />);
```

  </script>
</body>
</html>