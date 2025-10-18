<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Cosmic Explorer</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Arial', sans-serif;
            background: radial-gradient(ellipse at bottom, #1B2735 0%, #090A0F 100%);
            color: white;
            overflow-x: hidden;
            min-height: 100vh;
        }

        /* Starfield Background */
        #stars {
            position: fixed;
            width: 100%;
            height: 100%;
            z-index: -2;
        }

        .star {
            position: absolute;
            background: white;
            border-radius: 50%;
            animation: twinkle 3s infinite;
        }

        @keyframes twinkle {
            0%, 100% { opacity: 0.3; }
            50% { opacity: 1; }
        }

        /* Earth Background */
        #earth-container {
            position: fixed;
            right: -25%;
            top: 50%;
            transform: translateY(-50%);
            width: 50%;
            height: 80%;
            z-index: -1;
            transition: all 2s ease;
        }

        .earth {
            width: 100%;
            height: 100%;
            background: radial-gradient(circle at 30% 50%, #4A90E2 0%, #1a3d5c 50%, #0a1929 100%);
            border-radius: 50%;
            position: relative;
            box-shadow: 
                inset -30px -30px 50px rgba(0,0,0,0.5),
                inset 10px 10px 50px rgba(135, 206, 235, 0.3),
                0 0 100px rgba(135, 206, 235, 0.5);
            animation: rotate 30s linear infinite;
        }

        .earth::before {
            content: '';
            position: absolute;
            width: 100%;
            height: 100%;
            background: url('data:image/svg+xml,<svg xmlns="http://www.w3.org/2000/svg" viewBox="0 0 100 100"><path d="M20,50 Q30,30 40,50 T60,50 Q70,70 80,50" fill="none" stroke="%2332CD32" stroke-width="0.5" opacity="0.3"/></svg>') repeat;
            border-radius: 50%;
            opacity: 0.4;
        }

        @keyframes rotate {
            from { transform: rotate(0deg); }
            to { transform: rotate(360deg); }
        }

        /* Navigation */
        nav {
            position: relative;
            z-index: 100;
            padding: 30px;
            background: rgba(0, 0, 30, 0.7);
            backdrop-filter: blur(10px);
        }

        h1 {
            text-align: center;
            font-size: 3em;
            background: linear-gradient(45deg, #00d4ff, #090979);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
            margin-bottom: 20px;
            text-shadow: 0 0 30px rgba(0, 212, 255, 0.5);
        }

        .menu-buttons {
            display: flex;
            justify-content: center;
            gap: 30px;
            flex-wrap: wrap;
        }

        .menu-btn {
            padding: 15px 30px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            border: none;
            border-radius: 50px;
            color: white;
            font-size: 1.1em;
            cursor: pointer;
            transition: all 0.3s;
            box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        }

        .menu-btn:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 25px rgba(102, 126, 234, 0.5);
        }

        /* Solar System View */
        #solar-system {
            display: none;
            position: fixed;
            width: 100%;
            height: 100vh;
            top: 0;
            left: 0;
            z-index: 50;
            background: radial-gradient(ellipse at center, #0a0e27 0%, #000000 100%);
        }

        .sun {
            position: absolute;
            width: 80px;
            height: 80px;
            background: radial-gradient(circle, #FDB813 0%, #FFA000 70%);
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            box-shadow: 0 0 60px #FFA000;
        }

        .orbit {
            position: absolute;
            border: 1px solid rgba(255, 255, 255, 0.1);
            border-radius: 50%;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
        }

        .planet {
            position: absolute;
            border-radius: 50%;
            cursor: pointer;
            transition: transform 0.3s;
        }

        .planet:hover {
            transform: scale(1.5);
        }

        .mercury {
            width: 8px;
            height: 8px;
            background: #8C7853;
            animation: orbit-mercury 4s linear infinite;
        }

        .venus {
            width: 15px;
            height: 15px;
            background: #FFC649;
            animation: orbit-venus 7s linear infinite;
        }

        .earth-planet {
            width: 16px;
            height: 16px;
            background: #4A90E2;
            animation: orbit-earth 10s linear infinite;
        }

        .mars {
            width: 12px;
            height: 12px;
            background: #CD5C5C;
            animation: orbit-mars 15s linear infinite;
        }

        .jupiter {
            width: 35px;
            height: 35px;
            background: linear-gradient(#D4A574, #C88B3C);
            animation: orbit-jupiter 25s linear infinite;
        }

        .saturn {
            width: 30px;
            height: 30px;
            background: #FAD5A5;
            animation: orbit-saturn 30s linear infinite;
        }

        .uranus {
            width: 20px;
            height: 20px;
            background: #4FD0E0;
            animation: orbit-uranus 40s linear infinite;
        }

        .neptune {
            width: 19px;
            height: 19px;
            background: #4B70DD;
            animation: orbit-neptune 50s linear infinite;
        }

        @keyframes orbit-mercury {
            from { transform: rotate(0deg) translateX(60px) rotate(0deg); }
            to { transform: rotate(360deg) translateX(60px) rotate(-360deg); }
        }

        @keyframes orbit-venus {
            from { transform: rotate(0deg) translateX(90px) rotate(0deg); }
            to { transform: rotate(360deg) translateX(90px) rotate(-360deg); }
        }

        @keyframes orbit-earth {
            from { transform: rotate(0deg) translateX(120px) rotate(0deg); }
            to { transform: rotate(360deg) translateX(120px) rotate(-360deg); }
        }

        @keyframes orbit-mars {
            from { transform: rotate(0deg) translateX(150px) rotate(0deg); }
            to { transform: rotate(360deg) translateX(150px) rotate(-360deg); }
        }

        @keyframes orbit-jupiter {
            from { transform: rotate(0deg) translateX(220px) rotate(0deg); }
            to { transform: rotate(360deg) translateX(220px) rotate(-360deg); }
        }

        @keyframes orbit-saturn {
            from { transform: rotate(0deg) translateX(280px) rotate(0deg); }
            to { transform: rotate(360deg) translateX(280px) rotate(-360deg); }
        }

        @keyframes orbit-uranus {
            from { transform: rotate(0deg) translateX(340px) rotate(0deg); }
            to { transform: rotate(360deg) translateX(340px) rotate(-360deg); }
        }

        @keyframes orbit-neptune {
            from { transform: rotate(0deg) translateX(400px) rotate(0deg); }
            to { transform: rotate(360deg) translateX(400px) rotate(-360deg); }
        }

        /* Info Panel */
        .info-panel {
            display: none;
            position: fixed;
            right: 20px;
            top: 50%;
            transform: translateY(-50%);
            width: 350px;
            background: rgba(0, 0, 50, 0.9);
            backdrop-filter: blur(10px);
            padding: 30px;
            border-radius: 20px;
            border: 1px solid rgba(100, 200, 255, 0.3);
            z-index: 200;
            box-shadow: 0 0 30px rgba(0, 150, 255, 0.3);
        }

        .info-panel h2 {
            color: #00d4ff;
            margin-bottom: 15px;
            font-size: 2em;
        }

        .info-panel p {
            line-height: 1.6;
            margin-bottom: 10px;
            color: #e0e0e0;
        }

        .close-btn {
            position: absolute;
            top: 15px;
            right: 15px;
            background: none;
            border: none;
            color: white;
            font-size: 1.5em;
            cursor: pointer;
        }

        .back-btn {
            position: fixed;
            top: 30px;
            left: 30px;
            z-index: 100;
            padding: 10px 20px;
            background: rgba(100, 100, 255, 0.3);
            border: 1px solid rgba(255, 255, 255, 0.3);
            color: white;
            border-radius: 25px;
            cursor: pointer;
            display: none;
        }

        /* Stars Section */
        .stars-container {
            display: none;
            padding: 50px;
            max-width: 1200px;
            margin: 0 auto;
        }

        .star-card {
            background: rgba(20, 20, 60, 0.8);
            padding: 20px;
            margin: 20px 0;
            border-radius: 15px;
            border: 1px solid rgba(100, 150, 255, 0.3);
            cursor: pointer;
            transition: all 0.3s;
        }

        .star-card:hover {
            transform: translateX(10px);
            box-shadow: 0 5px 20px rgba(100, 150, 255, 0.5);
        }

        .star-card h3 {
            color: #ffd700;
            margin-bottom: 10px;
        }

        .star-card p {
            color: #b0b0b0;
        }
    </style>
</head>
<body>
    <!-- Starfield Background -->
    <div id="stars"></div>

    <!-- Earth Background -->
    <div id="earth-container">
        <div class="earth"></div>
    </div>

    <!-- Navigation -->
    <nav>
        <h1>🚀 Cosmic Explorer 🌌</h1>
        <div class="menu-buttons">
            <button class="menu-btn" onclick="showSolarSystem()">🪐 Planets</button>
            <button class="menu-btn" onclick="showStars()">⭐ Nearest Stars</button>
            <button class="menu-btn" onclick="showGalaxies()">🌀 Galaxies</button>
            <button class="menu-btn" onclick="showMissions()">🛸 Space Missions</button>
        </div>
    </nav>

    <!-- Back Button -->
    <button class="back-btn" onclick="goHome()">← Back to Home</button>

    <!-- Solar System View -->
    <div id="solar-system">
        <div class="sun"></div>
        
        <div class="orbit" style="width: 120px; height: 120px;"></div>
        <div class="orbit" style="width: 180px; height: 180px;"></div>
        <div class="orbit" style="width: 240px; height: 240px;"></div>
        <div class="orbit" style="width: 300px; height: 300px;"></div>
        <div class="orbit" style="width: 440px; height: 440px;"></div>
        <div class="orbit" style="width: 560px; height: 560px;"></div>
        <div class="orbit" style="width: 680px; height: 680px;"></div>
        <div class="orbit" style="width: 800px; height: 800px;"></div>

        <div class="planet mercury" onclick="showPlanetInfo('Mercury')"></div>
        <div class="planet venus" onclick="showPlanetInfo('Venus')"></div>
        <div class="planet earth-planet" onclick="showPlanetInfo('Earth')"></div>
        <div class="planet mars" onclick="showPlanetInfo('Mars')"></div>
        <div class="planet jupiter" onclick="showPlanetInfo('Jupiter')"></div>
        <div class="planet saturn" onclick="showPlanetInfo('Saturn')"></div>
        <div class="planet uranus" onclick="showPlanetInfo('Uranus')"></div>
        <div class="planet neptune" onclick="showPlanetInfo('Neptune')"></div>
    </div>

    <!-- Stars Container -->
    <div class="stars-container" id="stars-container">
        <h2 style="text-align: center; margin-bottom: 30px; color: #ffd700;">✨ Nearest Stars to Earth ✨</h2>
        
        <div class="star-card" onclick="showStarInfo('Proxima Centauri')">
            <h3>Proxima Centauri</h3>
            <p>Distance: 4.24 light-years</p>
            <p>Type: Red Dwarf</p>
        </div>

        <div class="star-card" onclick="showStarInfo('Alpha Centauri A & B')">
            <h3>Alpha Centauri A & B</h3>
            <p>Distance: 4.37 light-years</p>
            <p>Type: Binary Star System</p>
        </div>

        <div class="star-card" onclick="showStarInfo('Barnards Star')">
            <h3>Barnard's Star</h3>
            <p>Distance: 5.96 light-years</p>
            <p>Type: Red Dwarf</p>
        </div>

        <div class="star-card" onclick="showStarInfo('Wolf 359')">
            <h3>Wolf 359</h3>
            <p>Distance: 7.86 light-years</p>
            <p>Type: Red Dwarf</p>
        </div>

        <div class="star-card" onclick="showStarInfo('Sirius A & B')">
            <h3>Sirius A & B</h3>
            <p>Distance: 8.66 light-years</p>
            <p>Type: Binary Star System (Brightest star in night sky)</p>
        </div>
    </div>

    <!-- Info Panel -->
    <div class="info-panel" id="info-panel">
        <button class="close-btn" onclick="closeInfo()">×</button>
        <h2 id="info-title"></h2>
        <div id="info-content"></div>
    </div>

    <script>
        // Create starfield
        function createStars() {
            const starsContainer = document.getElementById('stars');
            for (let i = 0; i < 200; i++) {
                const star = document.createElement('div');
                star.className = 'star';
                star.style.width = Math.random() * 3 + 'px';
                star.style.height = star.style.width;
                star.style.left = Math.random() * 100 + '%';
                star.style.top = Math.random() * 100 + '%';
                star.style.animationDelay = Math.random() * 3 + 's';
                starsContainer.appendChild(star);
            }
        }

        createStars();

        // Planet Information Database
        const planetInfo = {
            'Mercury': {
                description: 'The smallest planet in our solar system and nearest to the Sun.',
                diameter: '4,879 km',
                dayLength: '59 Earth days',
                yearLength: '88 Earth days',
                moons: '0',
                temperature: '-173°C to 427°C',
                atmosphere: 'Minimal, mostly oxygen, sodium, hydrogen',
                funFact: 'A day on Mercury lasts longer than its year!'
            },
            'Venus': {
                description: 'The hottest planet in our solar system, often called Earth\'s twin.',
                diameter: '12,104 km',
                dayLength: '243 Earth days',
                yearLength: '225 Earth days',
                moons: '0',
                temperature: '462°C average',
                atmosphere: 'Carbon dioxide with sulfuric acid clouds',
                funFact: 'Venus rotates backwards compared to most planets!'
            },
            'Earth': {
                description: 'Our home planet, the only known planet with life.',
                diameter: '12,742 km',
                dayLength: '24 hours',
                yearLength: '365.25 days',
                moons: '1 (The Moon)',
                temperature: '-88°C to 58°C',
                atmosphere: 'Nitrogen (78%), Oxygen (21%)',
                funFact: 'Earth is the only planet not named after a god!'
            },
            'Mars': {
                description: 'The Red Planet, fourth from the Sun.',
                diameter: '6,779 km',
                dayLength: '24.6 hours',
                yearLength: '687 Earth days',
                moons: '2 (Phobos and Deimos)',
                temperature: '-143°C to 35°C',
                atmosphere: 'Mostly carbon dioxide',
                funFact: 'Mars has the largest volcano in the solar system - Olympus Mons!'
            },
            'Jupiter': {
                description: 'The largest planet in our solar system.',
                diameter: '139,820 km',
                dayLength: '10 hours',
                yearLength: '12 Earth years',
                moons: '79 known moons',
                temperature: '-108°C average',
                atmosphere: 'Hydrogen and helium',
                funFact: 'Jupiter\'s Great Red Spot is a storm larger than Earth!'
            },
            'Saturn': {
                description: 'Famous for its spectacular ring system.',
                diameter: '116,460 km',
                dayLength: '10.7 hours',
                yearLength: '29 Earth years',
                moons: '82 known moons',
                temperature: '-139°C average',
                atmosphere: 'Hydrogen and helium',
                funFact: 'Saturn could float in water because it\'s less dense than water!'
            },
            'Uranus': {
                description: 'The tilted ice giant, seventh from the Sun.',
                diameter: '50,724 km',
                dayLength: '17 hours',
                yearLength: '84 Earth years',
                moons: '27 known moons',
                temperature: '-197°C average',
                atmosphere: 'Hydrogen, helium, and methane',
                funFact: 'Uranus rotates on its side at a 98-degree angle!'
            },
            'Neptune': {
                description: 'The windiest planet in our solar system.',
                diameter: '49,244 km',
                dayLength: '16 hours',
                yearLength: '165 Earth years',
                moons: '14 known moons',
                temperature: '-201°C average',
                atmosphere: 'Hydrogen, helium, and methane',
                funFact: 'Neptune has winds that can reach 2,100 km/h!'
            }
        };

        // Star Information Database
        const starInfo = {
            'Proxima Centauri': {
                description: 'The nearest known star to the Sun, part of the Alpha Centauri system.',
                type: 'Red Dwarf (M-type)',
                mass: '0.12 solar masses',
                temperature: '3,042 K',
                luminosity: '0.0017 times the Sun',
                planets: 'At least 2 confirmed exoplanets',
                funFact: 'Proxima Centauri b, one of its planets, is in the habitable zone!'
            },
            'Alpha Centauri A & B': {
                description: 'A binary star system, the third brightest star in our night sky.',
                type: 'G-type (like our Sun) and K-type',
                mass: 'A: 1.1 solar masses, B: 0.91 solar masses',
                temperature: 'A: 5,790 K, B: 5,260 K',
                luminosity: 'Combined: 1.519 times the Sun',
                planets: 'Possible planets detected',
                funFact: 'These stars orbit each other every 80 years!'
            },
            'Barnards Star': {
                description: 'An ancient red dwarf, one of the oldest stars in the galaxy.',
                type: 'Red Dwarf (M-type)',
                mass: '0.144 solar masses',
                temperature: '3,134 K',
                luminosity: '0.0035 times the Sun',
                planets: 'At least 1 super-Earth exoplanet',
                funFact: 'It has the highest proper motion of any known star!'
            },
            'Wolf 359': {
                description: 'One of the nearest stars to Earth, very faint red dwarf.',
                type: 'Red Dwarf (M-type)',
                mass: '0.09 solar masses',
                temperature: '2,800 K',
                luminosity: '0.001 times the Sun',
                planets: 'At least 2 candidate planets',
                funFact: 'Featured in Star Trek as the location of a major battle!'
            },
            'Sirius A & B': {
                description: 'The brightest star in the night sky, actually a binary system.',
                type: 'A: Main sequence, B: White dwarf',
                mass: 'A: 2.06 solar masses, B: 1.02 solar masses',
                temperature: 'A: 9,940 K, B: 25,000 K',
                luminosity: 'A: 25.4 times the Sun',
                planets: 'None confirmed',
                funFact: 'Known as the "Dog Star" - it\'s part of Canis Major constellation!'
            }
        };

        function showSolarSystem() {
            document.getElementById('earth-container').style.transform = 'scale(0.1)';
            document.getElementById('earth-container').style.opacity = '0';
            setTimeout(() => {
                document.getElementById('solar-system').style.display = 'block';
                document.querySelector('.back-btn').style.display = 'block';
                document.querySelector('nav').style.display = 'none';
            }, 1000);
        }

        function showStars() {
            document.getElementById('earth-container').style.opacity = '0.3';
            document.getElementById('stars-container').style.display = 'block';
            document.querySelector('.back-btn').style.display = 'block';
            document.querySelector('nav').style.display = 'none';
        }

        function showPlanetInfo(planetName) {
            const info = planetInfo[planetName];
            document.getElementById('info-title').textContent = planetName;
            document.getElementById('info-content').innerHTML = `
                <p><strong>Overview:</strong> ${info.description}</p>
                <p><strong>Diameter:</strong> ${info.diameter}</p>
                <p><strong>Day Length:</strong> ${info.dayLength}</p>
                <p><strong>Year Length:</strong> ${info.yearLength}</p>
                <p><strong>Moons:</strong> ${info.moons}</p>
                <p><strong>Temperature:</strong> ${info.temperature}</p>
                <p><strong>Atmosphere:</strong> ${info.atmosphere}</p>
                <p style="color: #ffd700; margin-top: 15px;"><strong>Fun Fact:</strong> ${info.funFact}</p>
            `;
            document.getElementById('info-panel').style.display = 'block';
        }

        function showStarInfo(starName) {
            const info = starInfo[starName];
            document.getElementById('info-title').textContent = starName;
            document.getElementById('info-content').innerHTML = `
                <p><strong>Overview:</strong> ${info.description}</p>
                <p><strong>Star Type:</strong> ${info.type}</p>
                <p><strong>Mass:</strong> ${info.mass}</p>
                <p><strong>Temperature:</strong> ${info.temperature}</p>
                <p><strong>Luminosity:</strong> ${info.luminosity}</p>
                <p><strong>Known Planets:</strong> ${info.planets}</p>
                <p style="color: #ffd700; margin-top: 15px;"><strong>Fun Fact:</strong> ${info.funFact}</p>
            `;
            document.getElementById('info-panel').style.display = 'block';
        }

        function closeInfo() {
            document.getElementById('info-panel').style.display = 'none';
        }

        function goHome() {
            document.getElementById('solar-system').style.display = 'none';
            document.getElementById('stars-container').style.display = 'none';
            document.querySelector('.back-btn').style.display = 'none';
            document.querySelector('nav').style.display = 'block';
            document.getElementById('earth-container').style.transform = 'scale(1)';
            document.getElementById('earth-container').style.opacity = '1';
        }

        function showGalaxies() {
            alert('Galaxies section coming soon! 🌀');
        }

        function showMissions() {
            alert('Space Missions section coming soon! 🚀');
        }

        // Add keyboard navigation
        document.addEventListener('keydown', (e) => {
            if (e.key === 'Escape') {
                closeInfo();
                goHome();
            }
        });
    </script>
</body>
</html>
