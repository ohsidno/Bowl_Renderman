# RenderMan Ceramic Bowl Project 🥣

This project is a photorealistic render of a ceramic bowl, created using Pixar's RenderMan. The goal was to replicate a real-world object by focusing on detailed modeling, custom procedural shading with OSL, and realistic lighting.

The entire scene, including modeling, shading, lighting, and rendering, was defined and executed within a RenderMan `.rib` file.

---

## Key Features

* **Detailed Modeling**: The bowl is constructed from multiple geometric primitives, including spheres, a cylinder, and a torus, for an accurate shape.
* **Custom OSL Shaders**: The bowl's distinct two-tone appearance and speckled pattern were achieved using two custom OSL shaders (`OuterBowl` and `TwoToneBowl`). These shaders procedurally generate the brown base strap and the scattered dark dots with variable sizes.
* **Realistic Lighting**: The scene is lit using a combination of a `PxrDomeLight` with a high-dynamic-range image (HDRI) for soft, ambient light and a `PxrSphereLight` to create key specular highlights.
* **High-Quality Rendering**: Rendered at **4K resolution (3840x2160)** using the `PxrUnified` integrator with a low pixel variance threshold (0.001) to ensure a high-quality, noise-free image.
* **Depth of Field**: The camera setup includes a calculated depth of field to keep the bowl in sharp focus while gently blurring the background, drawing the viewer's attention.

---

## Project Breakdown

### 1. Modeling

The bowl was modeled by combining four distinct geometric primitives to accurately capture its form:
* **Outer Surface**: A large sphere (`radius 1`, scaled by `0.6`).
* **Inner Cavity**: A slightly smaller, offset sphere (`scaled by 0.585`).
* **Base**: A cylinder to create a sturdy, flat bottom.
* **Rim**: A thin torus to define the top edge of the bowl.

Aligning these primitives correctly was a key challenge to avoid seams and create a natural appearance. The table is a simple bilinear patch.

### 2. Shading and Texturing

The unique texture of the bowl was the main focus of the shading process.

* **`OuterBowl` Shader**: This OSL shader creates the two-tone effect on the exterior. It uses a height-based condition (`z <= -0.35`) to apply a **dark brown color** to the base and a **light gray** to the upper section.
* **`TwoToneBowl` Shader**: This shader applies a uniform light gray to the interior of the bowl.
* **Procedural Dots**: Both shaders use a procedural loop (400 iterations) with pseudo-random noise to scatter small, dark gray dots across the surface. The radius of each dot is varied to create a more organic, less uniform pattern.

The shaders were connected to a `PxrDisney` BxDF, with parameters fine-tuned to achieve a realistic ceramic gloss. The wooden table uses `PxrTexture` and `PxrNormalMap` to apply oak textures.

### 3. Lighting and Rendering

The lighting setup was designed to mimic a soft, indoor environment.

* **Ambient Light**: A `PxrDomeLight` with a high-dynamic-range image (HDRI) provides the main ambient illumination.
* **Key Light**: A `PxrSphereLight` was positioned to add direct light, creating distinct highlights on the bowl's rim and emphasizing its glossy finish.

The final scene was rendered in 4K using the `PxrUnified` integrator, with a camera configured for a 30-degree field of view and a shallow depth of field (f-stop of 100) to ensure the bowl was the central focus.

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
    The final image, `Bowl.tiff`, will be generated in the output directory.

---

## Future Improvements

While the final result is visually compelling, there are several areas for future improvement:
* **Model Thickness**: Refine the alignment of the inner and outer spheres for a more realistic bowl thickness.
* **Dot Distribution**: Improve the procedural dot pattern to avoid regularity, especially near the poles of the sphere.
* **Surface Imperfections**: Add a shader to the rim to simulate small chips or imperfections for enhanced realism.
