# Birds Music - A Procedural Audio-Visual Experience

A fully procedural, interactive audio-visual "relaxation toy" built with Three.js and its WebGPU renderer. It simulates a flock of thousands of birds (boids) whose movements generate a dynamic, generative musical soundscape. Users can interact with the flock using their mouse and customize nearly every aspect of the visual and flocking behavior through a detailed control panel.

## Requirements

*   **JavaScript (ES6+):** The browser must support JavaScript version 6 or higher.
*   **WebGPU:** The browser must support WebGPU. A modern version of Chrome or Edge is recommended for the best WebGPU support

## Features

*   **High-Performance WebGPU Simulation:** Utilizes WebGPU compute shaders via Three.js Shading Language (TSL) to simulate up to 32,768 birds smoothly on the GPU.
*   **Interactive Flocking:** Classic boids algorithm (Separation, Alignment, Cohesion) with additional parameters for "freedom" and "center attraction".
*   **Dynamic Mouse Interaction:** The mouse pointer acts as a repulsive force, allowing you to "disturb" the flock and influence its movement.
*   **Procedural Audio with Tone.js:**
    *   **Generative Soundscapes:** Five unique, selectable soundscapes (e.g., Ambient, Forest, Ocean) with different chord progressions, tempos, and synth textures.
    *   **Interactive Sound:**
        *   Flock activity level dynamically influences the background ambience
        *   Mouse movement speed generates a corresponding "wind" sound, enhancing the feeling of interaction.
*   **Advanced Visual Customization:**
    *   **Lighting:** Full control over a directional light (color, intensity, position) and an ambient light.
    *   **Procedural Skybox:** Customize the sky with gradient top/bottom colors and overall intensity.
    *   **Bird Material:** Switch between Matte, Glossy, and Metallic materials with adjustable shininess and opacity.
    *   **Bird Size:** Randomize the size of individual birds or reset them to a uniform scale.
*   **Comprehensive GUI:** A `lil-gui` panel provides real-time control over all simulation, lighting, material, and camera parameters.
*   **Preset System:** Save your favorite configurations as presets in your browser's local storage. Load, delete, and switch between them easily. Comes with several built-in presets to showcase different moods.
*   **Intelligent Camera:**
    *   Standard `OrbitControls` for manual navigation.
    *   An auto-rotation feature that gracefully kicks in after a few seconds of inactivity, turning the experience into a dynamic screensaver.

## Technical Breakdown

*   **Core Technologies:**
    *   **Three.js:** The foundational 3D library.
    *   **WebGPU Renderer:** Leverages the modern, high-performance WebGPU API for rendering and computation.
    *   **Three.js Shading Language (TSL):** Used to write the GPU-based bird simulation logic in a JavaScript-friendly way.
    *   **Tone.js:** A powerful Web Audio framework for creating the complex, scheduled, and interactive procedural music and sound effects.
    *   **lil-gui:** For the user-friendly control panel.

*   **Simulation Architecture:**
    *   The position and velocity of each bird are stored in GPU buffers (`instancedArray`).
    *   Two compute shaders are dispatched on every frame:
        1.  `computeVelocity`: Calculates the new velocity for each bird based on flocking rules, mouse interaction, and other forces.
        2.  `computePosition`: Updates the bird's position based on its new velocity.
    *   This entire simulation runs on the GPU, freeing the CPU to handle audio, UI, and other logic, enabling a very large number of birds.

*   **Rendering:**
    *   A single `BirdGeometry` is created for the bird model.
    *   The birds are rendered using a single draw call via GPU instancing.
    *   A custom `NodeMaterial` (written in TSL) reads the position, velocity, and scale for each instance from the GPU buffers to correctly position, orient, and scale each bird in the scene.

## How to Use & Explore

This application is designed for exploration and relaxation. Here are some ways to get started:

*   **Basic Interaction:** Simply move your mouse across the screen. You'll see the birds react to your pointer, and you'll hear the "wind" sound change with your movement speed.

*   **Camera:**
    *   **Click and drag** to rotate the view.
    *   **Right-click and drag** (or two-finger drag) to pan.
    *   **Scroll** to zoom in and out.
    *   **Stop moving the mouse** for a few seconds, and the camera will begin to rotate automatically.

*   **Audio Controls (Bottom-Left):**
    *   Click "Audio Controls" to expand the panel.
    *   Press "Play Music" to start the generative soundscape.
    *   Use the sliders to adjust the master volume, the volume of individual musical layers (Melody, Synth, Pad), and the amount of reverb.
    *   Use the "Soundscape" dropdown to completely change the musical theme.

*   **Simulation Controls (Top-Right):**
    *   Click the "Simulation Controls" bar to open the main GUI.
    *   **Presets:** The best way to start is by trying the different options in the "Load Preset" dropdown. This will show you the vast range of possible aesthetics.
    *   **Flock:** Adjust `Separation`, `Alignment`, and `Cohesion` to change how the birds fly together. Low separation and high alignment/cohesion create tight, flowing murmurations.
    *   **Lighting:** Play with the `Skybox`, `Directional Light`, and `Ambient Light` folders to become a virtual cinematographer. Change the time of day, create alien worlds, or go for a moody, dark atmosphere.
    *   **Bird Material:** Switch the `Type` to `Metallic` and increase the `Shininess` for a completely different look.
    *   **Randomize!** Don't be afraid to use the "Randomize" buttons in each section to discover unexpected and beautiful combinations. If you find one you like, give it a name in the "Presets" panel and save it!

## Credits

This project was inspired by the original [three.js birds examples](https://threejs.org/examples/?q=bird#webgpu_compute_birds) and further procedural audio/visual extensions were implemented by [sonicviz.com](https://sonicviz.com/us/).