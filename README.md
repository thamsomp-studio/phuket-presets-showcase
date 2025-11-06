<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Phuket Film Presets Showcase</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@400;600;700&display=swap" rel="stylesheet">
    <!-- Chosen Palette: Muted Cinematic (Warm Neutrals, Desaturated Teal/Brown Accents) -->
    <!-- Application Structure Plan: The SPA is a high-conversion product landing page. It follows a vertical flow: Value Proposition -> Interactive Before/After Showcase -> Quantitative Feature Breakdown -> AI Feature 1 (Aesthetics) -> AI Feature 2 (Tips) -> Local Expertise USP. -->
    <!-- Visualization & Content Choices: 
    1. Aesthetic Shift (Before/After). Interactive Slider/Toggle.
    2. Preset 'Recipe' (Grain, Contrast, Tones). Horizontal Bar Chart using Chart.js.
    3. AI Feature 1: Uses Gemini API (gemini-2.5-flash-preview-09-2025) to generate a descriptive film aesthetic suggestion based on user input scene.
    4. AI Feature 2: Uses Gemini API (gemini-2.5-flash-preview-09-2025) to generate a Phuket-specific photography tip based on user challenge.
    -->
    <style>
        body {
            font-family: 'Inter', sans-serif;
        }
        .bna-container {
            min-height: 200px;
        }
        .chart-container {
            position: relative;
            width: 100%;
            max-width: 700px;
            margin-left: auto;
            margin-right: auto;
            height: 250px;
            max-height: 300px;
        }
        @media (min-width: 768px) {
            .chart-container {
                height: 300px;
            }
        }
    </style>
</head>
<body class="bg-stone-50 text-stone-800">

    <div class="container mx-auto max-w-4xl p-4 md:p-8">

        <header class="text-center py-12 mb-8 bg-white rounded-xl shadow-lg border border-stone-200">
            <h1 class="text-4xl md:text-5xl font-extrabold text-amber-900 mb-3">
                <span class="text-stone-500">🎞️</span> The Phuket Film Preset Collection
            </h1>
            <p class="text-xl text-stone-600 max-w-2xl mx-auto">
                Give your photos the coveted vintage film aesthetic—perfected on the vibrant, diverse lighting of Thailand's Andaman Coast.
            </p>
        </header>

        <main class="space-y-12">

            <section id="interactive-showcase" class="bg-white p-6 md:p-8 rounded-xl shadow-lg border border-stone-200">
                <h2 class="text-3xl font-bold text-amber-900 mb-6">Interactive Film Look Generator</h2>
                <p class="text-stone-600 mb-6">
                    This interactive section lets you explore the powerful transformation of the presets. Drag the slider to transition between the clean **Modern Digital** look and the soulful **Vintage Film** aesthetic.
                </p>

                <div class="flex flex-col md:flex-row justify-center gap-4 bna-container">
                    
                    <div id="before-box" class="flex-1 p-6 rounded-lg border-2 border-stone-300 transition-all duration-500 bg-teal-50">
                        <h3 class="text-xl font-bold text-stone-700 mb-2">Modern Digital (Before)</h3>
                        <div id="before-status" class="flex items-center text-sm font-medium text-stone-500 space-x-2">
                            <span>Clarity: High</span>
                            <span>Grain: Absent</span>
                        </div>
                    </div>

                    <div id="after-box" class="flex-1 p-6 rounded-lg border-2 border-amber-500 transition-all duration-500 bg-amber-50">
                        <h3 class="text-xl font-bold text-amber-900 mb-2">Vintage Film (After)</h3>
                        <div id="after-status" class="flex items-center text-sm font-medium text-amber-700 space-x-2">
                            <span>Mood: Rich & Moody</span>
                            <span>Grain: Subtle</span>
                        </div>
                    </div>
                </div>

                <div class="mt-8">
                    <input type="range" min="0" max="100" value="100" class="w-full h-2 bg-stone-300 rounded-lg appearance-none cursor-pointer" id="filmSlider">
                    <div class="flex justify-between text-sm text-stone-600 mt-2">
                        <span>Flat & Clean</span>
                        <span>Atmospheric & Nostalgic</span>
                    </div>
                </div>
                
                <p id="description-output" class="text-stone-700 text-center mt-6 p-4 bg-stone-100 rounded-lg border border-stone-200 font-semibold">
                    The presets are currently showing the **Vintage Film Look**—characterized by rich, moody colors and subtle, authentic film grain.
                </p>

            </section>

            <section id="feature-breakdown" class="bg-white p-6 md:p-8 rounded-xl shadow-lg border border-stone-200">
                <h2 class="text-3xl font-bold text-amber-900 mb-6">The Film Recipe: Key Preset Features</h2>
                <p class="text-stone-600 mb-6">
                    These presets are a result of meticulous adjustments to mimic traditional analog film stocks. The chart below shows the intensity of the three primary components that define this vintage aesthetic.
                </p>
                <div class="chart-container">
                    <canvas id="presetChart"></canvas>
                </div>
                <div class="mt-6 text-center text-sm text-stone-500">
                    <p>Features are rated on an intensity scale of 1 to 10 for the default preset settings.</p>
                </div>
            </section>

            <!-- Gemini AI Integration 1: Film Storyteller AI -->
            <section id="gemini-feature-1" class="bg-amber-50 p-6 md:p-8 rounded-xl shadow-xl border-4 border-amber-300">
                <h2 class="text-3xl font-bold text-amber-900 mb-4">✨ Film Storyteller AI</h2>
                <p class="text-stone-700 mb-6">
                    Describe the scene or story you want to capture (e.g., "Golden hour portrait near a fishing boat," or "Moody street food scene in the rain"). Our AI will instantly suggest the perfect vintage film aesthetic for your image.
                </p>

                <textarea id="sceneInput" rows="3" class="w-full p-3 border-2 border-stone-300 rounded-lg focus:border-amber-500 transition-all text-stone-800" placeholder="Describe your photo here..."></textarea>

                <button id="suggestButton" class="w-full bg-amber-700 text-white font-bold py-3 mt-4 rounded-lg text-lg shadow-md hover:bg-amber-800 transition-colors flex items-center justify-center">
                    <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white hidden" id="loadingSpinner1" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                        <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                        <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                    </svg>
                    ✨ Suggest Vintage Film Look
                </button>
                
                <div id="resultOutput1" class="mt-6 p-4 bg-white rounded-lg border-2 border-stone-200 text-stone-800 hidden">
                    <h4 class="font-bold text-lg mb-2 text-amber-900">Suggested Film Aesthetic:</h4>
                    <p id="suggestionText1" class="text-stone-700 italic"></p>
                    <p id="suggestionSource1" class="text-xs text-stone-500 mt-2"></p>
                </div>
            </section>
            <!-- End Gemini AI Integration 1 -->


            <!-- Gemini AI Integration 2: Local Photography Tip AI (Targeted Section) -->
            <section id="island-tested" class="bg-white p-6 md:p-8 rounded-xl shadow-lg border border-stone-200">
                <h2 class="text-3xl font-bold text-amber-900 mb-6">Island-Tested Expertise</h2>
                <p class="text-stone-600 mb-4">
                    The core value you're purchasing is **local expertise**. I'm a photographer and visual storyteller based right here on Phuket Island. These presets haven't just been created—they have been perfected under the unique, challenging, and vibrant lighting of the Andaman region.
                </p>

                <div id="gemini-feature-2" class="bg-teal-50 p-5 rounded-xl shadow-md border-2 border-teal-300 mb-8">
                    <h3 class="text-2xl font-bold text-teal-800 mb-3">✨ Local Photography Tip AI</h3>
                    <p class="text-stone-700 mb-4 text-sm">
                        Facing a difficult shoot condition in Phuket? Ask our AI for a pro tip (e.g., "how to shoot Kata Beach at noon," or "dealing with low-light temples").
                    </p>

                    <input type="text" id="tipInput" class="w-full p-2 border border-stone-300 rounded-lg focus:border-teal-500 transition-all text-stone-800" placeholder="Ask for a Phuket photography tip...">

                    <button id="tipButton" class="w-full bg-teal-700 text-white font-bold py-2 mt-3 rounded-lg text-md shadow-md hover:bg-teal-800 transition-colors flex items-center justify-center">
                        <svg class="animate-spin -ml-1 mr-3 h-5 w-5 text-white hidden" id="loadingSpinner2" xmlns="http://www.w3.org/2000/svg" fill="none" viewBox="0 0 24 24">
                            <circle class="opacity-25" cx="12" cy="12" r="10" stroke="currentColor" stroke-width="4"></circle>
                            <path class="opacity-75" fill="currentColor" d="M4 12a8 8 0 018-8V0C5.373 0 0 5.373 0 12h4zm2 5.291A7.962 7.962 0 014 12H0c0 3.042 1.135 5.824 3 7.938l3-2.647z"></path>
                        </svg>
                        Get Pro Tip
                    </button>
                    
                    <div id="resultOutput2" class="mt-4 p-3 bg-white rounded-lg border-2 border-stone-200 text-stone-800 hidden">
                        <h4 class="font-bold text-md mb-1 text-teal-800">Pro Tip:</h4>
                        <p id="tipText" class="text-stone-700 text-sm"></p>
                    </div>
                </div>
                <!-- End Gemini AI Integration 2 -->

                <div class="grid md:grid-cols-3 gap-6 mt-6">
                    <div class="p-4 bg-amber-50 rounded-lg border border-amber-200 text-center">
                        <p class="text-2xl mb-1">☀️</p>
                        <p class="font-bold text-amber-900">Tropical Light Mastery</p>
                        <p class="text-sm text-stone-600">Handles harsh midday sun and soft sunset gold perfectly.</p>
                    </div>
                    <div class="p-4 bg-amber-50 rounded-lg border border-amber-200 text-center">
                        <p class="text-2xl mb-1">🌊</p>
                        <p class="font-bold text-amber-900">Authentic Blue Tones</p>
                        <p class="text-sm text-stone-600">Gives ocean and sky blues that rich, soulful depth.</p>
                    </div>
                    <div class="p-4 bg-amber-50 rounded-lg border border-amber-200 text-center">
                        <p class="text-2xl mb-1">🥥</p>
                        <p class="font-bold text-amber-900">Vibrant Local Colors</p>
                        <p class="text-sm text-stone-600">Ensures vibrant island greens and reds retain character.</p>
                    </div>
                </div>
                
                <div class="mt-8 text-center">
                    <button class="bg-amber-900 text-white font-bold py-3 px-8 rounded-full text-xl shadow-xl hover:bg-amber-800 transition-colors">
                        Ready for a Soulful Upgrade? Purchase Now!
                    </button>
                </div>
            </section>

        </main>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {

            const slider = document.getElementById('filmSlider');
            const beforeBox = document.getElementById('before-box');
            const afterBox = document.getElementById('after-box');
            const descriptionOutput = document.getElementById('description-output');
            
            // --- Helper Functions ---
            const apiKey = "";
            const apiUrl = `https://generativelanguage.googleapis.com/v1beta/models/gemini-2.5-flash-preview-09-2025:generateContent?key=${apiKey}`;

            /**
             * Handles common fetch logic with exponential backoff.
             */
            async function callGeminiApi(systemPrompt, userQuery) {
                const payload = {
                    contents: [{ parts: [{ text: userQuery }] }],
                    tools: [{ "google_search": {} }],
                    systemInstruction: {
                        parts: [{ text: systemPrompt }]
                    },
                };

                let response = null;
                let retries = 0;
                const maxRetries = 5;

                while (retries < maxRetries) {
                    try {
                        const fetchResponse = await fetch(apiUrl, {
                            method: 'POST',
                            headers: { 'Content-Type': 'application/json' },
                            body: JSON.stringify(payload)
                        });

                        if (fetchResponse.status === 429) {
                            const delay = Math.pow(2, retries) * 1000;
                            retries++;
                            console.warn(`Rate limit hit. Retrying in ${delay}ms...`);
                            await new Promise(resolve => setTimeout(resolve, delay));
                            continue;
                        }

                        if (!fetchResponse.ok) {
                            throw new Error(`API request failed with status: ${fetchResponse.status}`);
                        }

                        response = await fetchResponse.json();
                        break;

                    } catch (error) {
                        console.error("Fetch error:", error);
                        throw new Error("Failed to connect to the AI service.");
                    }
                }

                if (!response) {
                    throw new Error("Maximum retries reached or request failed.");
                }
                
                const candidate = response.candidates?.[0];
                const text = candidate?.content?.parts?.[0]?.text;

                if (!text) {
                     throw new Error("AI generated an empty or malformed response.");
                }

                // Extract grounding sources
                let sources = [];
                const groundingMetadata = candidate.groundingMetadata;
                if (groundingMetadata && groundingMetadata.groundingAttributions) {
                    sources = groundingMetadata.groundingAttributions
                        .map(attribution => ({
                            uri: attribution.web?.uri,
                            title: attribution.web?.title,
                        }))
                        .filter(source => source.uri && source.title);
                }

                const sourcesHtml = sources.length > 0
                    ? '<span class="font-medium">Sources:</span> ' + sources.map(s => `<a href="${s.uri}" target="_blank" class="hover:underline text-amber-600">${s.title}</a>`).join(', ')
                    : '';

                return { text, sourcesHtml };
            }

            // --- UI/UX Logic ---

            function updateLook(value) {
                const normalized = value / 100; 

                const beforeOpacity = 1 - normalized;
                const afterOpacity = normalized;

                beforeBox.style.opacity = beforeOpacity;
                afterBox.style.opacity = afterOpacity;

                if (value === 0) {
                    descriptionOutput.innerHTML = 'The presets are currently showing the **Modern Digital Look**—clean, high clarity, and ready for minimal post-processing.';
                    beforeBox.classList.add('border-teal-500');
                    beforeBox.classList.remove('border-stone-300');
                    afterBox.classList.remove('border-amber-500');
                    afterBox.classList.add('border-stone-300');
                } else if (value === 100) {
                    descriptionOutput.innerHTML = 'The presets are currently showing the **Vintage Film Look**—characterized by rich, moody colors and subtle, authentic film grain.';
                    afterBox.classList.add('border-amber-500');
                    afterBox.classList.remove('border-stone-300');
                    beforeBox.classList.remove('border-teal-500');
                    beforeBox.classList.add('border-stone-300');
                } else {
                    descriptionOutput.innerHTML = 'Drag the slider to the ends to see the full **Modern vs. Vintage** effect.';
                    beforeBox.classList.remove('border-teal-500');
                    afterBox.classList.remove('border-amber-500');
                    beforeBox.classList.add('border-stone-300');
                    afterBox.classList.add('border-stone-300');
                }
            }

            slider.addEventListener('input', (event) => {
                updateLook(parseInt(event.target.value));
            });

            updateLook(100); 

            // --- Chart Logic ---

            const ctx = document.getElementById('presetChart').getContext('2d');
            const presetChart = new Chart(ctx, {
                type: 'bar',
                data: {
                    labels: ['Rich, Moody Tones', 'Classic Tone Curve', 'Subtle Film Grain', 'Desaturated Greens'],
                    datasets: [{
                        label: 'Intensity (1-10)',
                        data: [9, 8, 7, 7],
                        backgroundColor: 'rgba(120, 53, 15, 0.8)', 
                        borderColor: 'rgba(120, 53, 15, 1)',
                        borderWidth: 1,
                        borderRadius: 4
                    }]
                },
                options: {
                    indexAxis: 'y',
                    responsive: true,
                    maintainAspectRatio: false,
                    scales: {
                        x: {
                            max: 10,
                            min: 0,
                            ticks: {
                                stepSize: 2
                            },
                            grid: {
                                display: true,
                                color: 'rgba(200, 200, 200, 0.3)'
                            },
                            title: {
                                display: true,
                                text: 'Feature Strength'
                            }
                        },
                        y: {
                            grid: {
                                display: false
                            }
                        }
                    },
                    plugins: {
                        legend: {
                            display: false
                        },
                        tooltip: {
                            callbacks: {
                                label: function(context) {
                                    return `${context.label}: Strength ${context.parsed.x}/10`;
                                }
                            }
                        }
                    }
                }
            });


            // --- GEMINI API INTEGRATION 1: Film Storyteller AI ---
            const suggestButton = document.getElementById('suggestButton');
            const loadingSpinner1 = document.getElementById('loadingSpinner1');
            const sceneInput = document.getElementById('sceneInput');
            const resultOutput1 = document.getElementById('resultOutput1');
            const suggestionText1 = document.getElementById('suggestionText1');
            const suggestionSource1 = document.getElementById('suggestionSource1');

            suggestButton.addEventListener('click', async () => {
                const scene = sceneInput.value.trim();
                if (scene === '') {
                    console.error('Validation Error: Please describe your photo scene first!');
                    sceneInput.classList.add('border-red-500');
                    setTimeout(() => sceneInput.classList.remove('border-red-500'), 2000);
                    return;
                }

                // UI state: Loading
                suggestButton.disabled = true;
                loadingSpinner1.classList.remove('hidden');
                resultOutput1.classList.remove('bg-red-100', 'border-red-500', 'border-stone-200');
                resultOutput1.classList.add('border-amber-500', 'hidden');
                suggestionText1.textContent = '';
                suggestionSource1.textContent = '';
                
                try {
                    const systemPrompt = "You are a world-class vintage film photography consultant specializing in visual aesthetics. Analyze the user's scene description and provide a concise (2-3 sentences), highly descriptive suggestion for the required film aesthetic, focusing on tones, grain, contrast, and overall mood. Do NOT use markdown formatting (like ** or *). Do NOT mention the word 'Lightroom' or 'preset'.";
                    const userQuery = `Analyze this photo scene and suggest the ideal vintage film look: "${scene}"`;

                    const { text, sourcesHtml } = await callGeminiApi(systemPrompt, userQuery);
                    
                    suggestionText1.textContent = text;
                    suggestionSource1.innerHTML = sourcesHtml;
                    resultOutput1.classList.remove('hidden', 'border-amber-500');
                    resultOutput1.classList.add('border-stone-200');

                } catch (error) {
                    console.error("AI Generation Error (Aesthetic):", error);
                    suggestionText1.textContent = `Error: ${error.message}. Please try again.`;
                    resultOutput1.classList.remove('hidden', 'border-amber-500');
                    resultOutput1.classList.add('bg-red-100', 'border-red-500');

                } finally {
                    // UI state: Complete
                    suggestButton.disabled = false;
                    loadingSpinner1.classList.add('hidden');
                }
            });


            // --- GEMINI API INTEGRATION 2: Local Photography Tip AI ---
            const tipButton = document.getElementById('tipButton');
            const loadingSpinner2 = document.getElementById('loadingSpinner2');
            const tipInput = document.getElementById('tipInput');
            const resultOutput2 = document.getElementById('resultOutput2');
            const tipText = document.getElementById('tipText');

            tipButton.addEventListener('click', async () => {
                const challenge = tipInput.value.trim();
                if (challenge === '') {
                    console.error('Validation Error: Please ask for a photography tip first!');
                    tipInput.classList.add('border-red-500');
                    setTimeout(() => tipInput.classList.remove('border-red-500'), 2000);
                    return;
                }

                // UI state: Loading
                tipButton.disabled = true;
                loadingSpinner2.classList.remove('hidden');
                resultOutput2.classList.remove('bg-red-100', 'border-red-500', 'border-stone-200');
                resultOutput2.classList.add('border-teal-500', 'hidden');
                tipText.textContent = '';
                
                try {
                    const systemPrompt = "You are an experienced photographer based in Phuket, Thailand. Provide a concise, professional, and practical photography tip (1-2 sentences) tailored to the user's specific challenge, referencing the local environment (e.g., intense sun, specific locations like temples or beaches). Focus on camera settings, gear, or composition. Do NOT use markdown formatting (like ** or *).";
                    const userQuery = `Provide a photography tip for this Phuket-related challenge: "${challenge}"`;

                    const { text } = await callGeminiApi(systemPrompt, userQuery);
                    
                    tipText.textContent = text;
                    resultOutput2.classList.remove('hidden', 'border-teal-500');
                    resultOutput2.classList.add('border-stone-200');

                } catch (error) {
                    console.error("AI Generation Error (Tip):", error);
                    tipText.textContent = `Error: ${error.message}. Please try again.`;
                    resultOutput2.classList.remove('hidden', 'border-teal-500');
                    resultOutput2.classList.add('bg-red-100', 'border-red-500');

                } finally {
                    // UI state: Complete
                    tipButton.disabled = false;
                    loadingSpinner2.classList.add('hidden');
                }
            });
        });
    </script>
</body>
</html>
