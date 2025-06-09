# DiffBIR – Blind Image Restoration with Generative Diffusion Prior

Implementación oficial de DiffBIR, un pipeline híbrido para **restauración ciega de imágenes** que combina una CNN ligera de pre-restauración con un ControlNet de difusión latente para generar detalles finos y texturas realistas de forma controlada.

---

## Métodos Destacados

| Método                         | Arquitectura / Pipeline                                                                                              | Tareas Principales                                | Datasets (Entreno / Prueba)                                                   |
|--------------------------------|----------------------------------------------------------------------------------------------------------------------|---------------------------------------------------|-------------------------------------------------------------------------------|
| [**DiffBIR** (ECCV 2024)](https://0x3f3f3f3fun.github.io/projects/diffbir/)        | Two-stage: CNN ligera para pre-restauración + IRControlNet (Stable Diffusion + ControlNet) con guía adaptativa.    | Blind SR, blind denoising, blind face restoration. | DF2K/FFHQ sintético → fine-tune SD1.5 → Eval: RealSR, Urban100, BSRBench, SIDD, DND, CelebA-HQ, LFW |
| [**Restormer** (CVPR 2022)](https://doi.org/10.1109/CVPR52688.2022.00564)      | Transformer jerárquico con MDTA (atención por canales) y GDFN (FFN gateado) para capturar dependencia global.       | Derain, motion & defocus deblurring, denoising.    | Rain13K/Rain100H; GoPro/HIDE/RealBlur-J/B; SIDD + DND                                    |
| [**Real-ESRGAN** (ICCVW 2021)](https://arxiv.org/abs/2107.10833)     | CNN RRDB + GAN con “degradation shuffle” (blur, ruido, compresión, ringing) y discriminador U-Net.                   | Blind super-resolución (×2, ×4).                  | DIV2K+Flickr2K+OST sintético; RealSR, DRealSR, DPED, OST300                                |


Aunque **Restormer** y **Real-ESRGAN** lideran en sus dominios (Transformer global y SR rápido con GAN), **DiffBIR** destaca al **unificar** tres tareas ciegas en un solo modelo, combinar **fidelidad estructural** con **detalles generativos** controlables y lograr resultados SOTA en PSNR, LPIPS y FID. Este diseño marca el camino para futuras soluciones de blind image restoration.  

---

## Artículos revisados

| Referencia                                                     | Año  | Descripción breve                                                                                                                         | Enlace                                                    |
|----------------------------------------------------------------|------|-------------------------------------------------------------------------------------------------------------------------------------------|-----------------------------------------------------------|
| Dabov *et al.*, **BM3D** – IEEE TIP                            | 2007 | Block matching y filtrado colaborativo 3D para denoising no supervisado.                                                                  | https://doi.org/10.1109/TIP.2007.901238                   |
| Zhang *et al.*, **DnCNN** – IEEE TIP                           | 2017 | CNN residual profunda para *blind denoising*, capaz de manejar múltiples niveles de ruido con un solo modelo.                             | https://doi.org/10.1109/TIP.2017.2662206                  |
| Zamir *et al.*, **MPRNet** – CVPR                              | 2021 | Arquitectura multi-stage progresiva (3 etapas) para derain, deblur y denoise, refinando contexto y detalle local en cada paso.             | https://doi.org/10.1109/CVPR46437.2021.01458              |
| Wang *et al.*, **GFP-GAN** – CVPR                              | 2022 | Restauración facial ciega con prior generativo, centrado en reconstruir detalles de rostros reales mediante GAN.                          | https://doi.org/10.1109/CVPR52688.2022.00511              |
| Zhang *et al.*, **BSRGAN** – ICCV                              | 2021 | Modelo práctico de degradación aleatoria para super-resolución ciega, simulando blur, ruido y compresión en diverso orden.                 | https://doi.org/10.1109/ICCV48922.2021.00475              |
| Wang *et al.*, **Real-ESRGAN** – ICCVW                         | 2021 | SR ciega real con degradaciones de orden ≥2 via “degradation shuffle” y GAN, entrenado solo con datos sintéticos.                         | https://arxiv.org/abs/2107.10833                          |
| Chen *et al.*, **NAFNet** – ECCV Workshops                     | 2022 | Red CNN sin activaciones no lineales innecesarias, SOTA en deblur y denoise con muy bajo coste computacional.                              | https://doi.org/10.1007/978-3-031-20071-7_2                |
| Chen *et al.*, **IPT** – CVPR                                  | 2021 | Pre-Trained Image Processing Transformer: gran Transformer multi-tarea entrenado en degradaciones sintéticas y fine-tuned por tarea.      | https://doi.org/10.1109/CVPR46437.2021.01212              |
| Liang *et al.*, **SwinIR** – ICCV Workshops                    | 2021 | CNN + Swin Transformer con atención local en ventanas desplazadas, mejora PSNR/SSIM en denoise, SR y eliminación de artifacts JPEG.      | https://doi.org/10.1109/ICCVW54120.2021.00210             |
| Wang *et al.*, **Uformer** – CVPR                              | 2022 | U-Shaped Transformer con atención en ventanas y moduladores multiescala, restauración general (denoise, deblur, derain, demoire).         | https://doi.org/10.1109/CVPR52688.2022.01716              |
| Zamir *et al.*, **Restormer** – CVPR (Oral)                    | 2022 | Efficient Transformer para restauración de alta resolución: MDTA + GDFN para derain, deblur y denoise real/sint.                           | https://doi.org/10.1109/CVPR52688.2022.00564              |
| Zhou *et al.*, **CodeFormer** – NeurIPS                        | 2022 | Restauración ciega de rostros via Transformer con código VQGAN, robusto frente a degradaciones severas.                                    | https://arxiv.org/abs/2206.11253                          |
| Lugmayr *et al.*, **RePaint** – CVPR                           | 2022 | Inpainting con DDPM no condicional, rellena máscaras arbitrarias sin reentrenar el modelo generativo.                                      | https://doi.org/10.1109/CVPR52688.2022.01117              |
| Saharia *et al.*, **Palette** – SIGGRAPH                       | 2022 | Difusor condicional único para colorización, inpainting, uncropping y JPEG-restoration; generaliza múltiples tareas sin ajustes.         | https://doi.org/10.1145/3528233.3530757                   |
| Zhang *et al.*, **DiffTSR** – CVPR                             | 2024 | Super-resolución ciega de texto en imágenes con difusión guiada por OCR, mejora legibilidad y calidad visual del texto embebido.         | https://doi.org/10.1109/CVPR52733.2024.02440              |
| **Lin *et al.*, DiffBIR** – ECCV                               | 2024 | **Two-stage**: CNN ligera (pre-restauración) + Diffusion ControlNet (Stable Diffusion) con guía adaptativa; unifica SR, denoise y BFR.   | https://doi.org/10.1109/CVPR52733.2024.02440              |
