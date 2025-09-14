# RenderMan Ceramic Bowl Project 🥣

This project is a photorealistic render of a ceramic bowl, created using Pixar's RenderMan. The goal was to replicate a real-world object by focusing on detailed modeling, custom procedural shading with OSL, and realistic lighting.

The entire scene, including modeling, shading, lighting, and rendering, was defined and executed within a RenderMan `.rib` file.

---

## Key Features

* [cite_start]**Detailed Modeling**: The bowl is constructed from multiple geometric primitives, including spheres, a cylinder, and a torus, for an accurate shape[cite: 30].
* [cite_start]**Custom OSL Shaders**: The bowl's distinct two-tone appearance and speckled pattern were achieved using two custom OSL shaders (`OuterBowl` and `TwoToneBowl`)[cite: 45]. [cite_start]These shaders procedurally generate the brown base strap and the scattered dark dots with variable sizes[cite: 46, 47].
* [cite_start]**Realistic Lighting**: The scene is lit using a combination of a `PxrDomeLight` with an HDRI from Poly Haven for soft, ambient light and a `PxrSphereLight` to create key specular highlights[cite: 62].
* [cite_start]**High-Quality Rendering**: Rendered at **4K resolution (3840x2160)** using the `PxrUnified` integrator with a low pixel variance threshold (0.001) to ensure a high-quality, noise-free image[cite: 69, 70].
* [cite_start]**Depth of Field**: The camera setup includes a calculated depth of field to keep the bowl in sharp focus while gently blurring the background, drawing the viewer's attention[cite: 71, 76].

---

## Project Breakdown

### 1. Modeling

[cite_start]The bowl was modeled by combining four distinct geometric primitives to accurately capture its form[cite: 29]:
* [cite_start]**Outer Surface**: A large sphere (`radius 1`, scaled by `0.6`)[cite: 30].
* [cite_start]**Inner Cavity**: A slightly smaller, offset sphere (`scaled by 0.585`)[cite: 30].
* [cite_start]**Base**: A cylinder to create a sturdy, flat bottom[cite: 30].
* [cite_start]**Rim**: A thin torus to define the top edge of the bowl[cite: 30].

[cite_start]Aligning these primitives correctly was a key challenge to avoid seams and create a natural appearance[cite: 31, 37]. [cite_start]The table is a simple bilinear patch[cite: 38].

### 2. Shading and Texturing

The unique texture of the bowl was the main focus of the shading process.

* **`OuterBowl` Shader**: This OSL shader creates the two-tone effect on the exterior. [cite_start]It uses a height-based condition (`z <= -0.35`) to apply a **dark brown color** to the base and a **light gray** to the upper section[cite: 46].
* [cite_start]**`TwoToneBowl` Shader**: This shader applies a uniform light gray to the interior of the bowl[cite: 49].
* [cite_start]**Procedural Dots**: Both shaders use a procedural loop (400 iterations) with pseudo-random noise to scatter small, dark gray dots across the surface[cite: 46]. [cite_start]The radius of each dot is varied to create a more organic, less uniform pattern[cite: 46, 47].

[cite_start]The shaders were connected to a `PxrDisney` BxDF, with parameters fine-tuned to achieve a realistic ceramic gloss[cite: 51]. [cite_start]The wooden table uses `PxrTexture` and `PxrNormalMap` to apply oak textures[cite: 52].

### 3. Lighting and Rendering

The lighting setup was designed to mimic a soft, indoor environment.

* [cite_start]**Ambient Light**: A `PxrDomeLight` with the "Lythwood Lounge" HDRI from Poly Haven provides the main ambient illumination[cite: 62, 103].
* [cite_start]**Key Light**: A `PxrSphereLight` was positioned to add direct light, creating distinct highlights on the bowl's rim and emphasizing its glossy finish[cite: 62].

[cite_start]The final scene was rendered in 4K using the `PxrUnified` integrator, with a camera configured for a 30-degree field of view and a shallow depth of field (f-stop of 100) to ensure the bowl was the central focus[cite: 69, 71].

---

## How to Render

To render this project, you'll need Pixar's RenderMan installed and configured.

1.  **Clone the repository:**
    ```bash
    git clone <your-repo-url>
    cd <your-repo-folder>
    ```
2.  **Compile the OSL shaders:**
    Make sure the `OuterBowl.osl` and `TwoToneBowl.osl` shaders are compiled to `.oso` files.
3.  **Render the scene:**
    Open a terminal that is configured for RenderMan and run the following command on the `.rib` file.
    ```bash
    prman Bowl.rib
    ```
    [cite_start]The final image, `Bowl.tiff`, will be generated in the output directory[cite: 70].

---

## Future Improvements

While the final result is visually compelling, there are several areas for future improvement:
* [cite_start]**Model Thickness**: Refine the alignment of the inner and outer spheres for a more realistic bowl thickness[cite: 87].
* [cite_start]**Dot Distribution**: Improve the procedural dot pattern to avoid regularity, especially near the poles of the sphere[cite: 89].
* [cite_start]**Surface Imperfections**: Add a shader to the rim to simulate small chips or imperfections for enhanced realism[cite: 90].
