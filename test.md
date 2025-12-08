# Track Anything Annotate 🚀

<div align="center">

  <a href="https://arxiv.org/abs/2505.17884">
    <img src="https://img.shields.io/badge/📄-Arxiv_Paper-B31B1B.svg?style=flat-square&logo=arxiv&logoColor=white" alt="Paper on arXiv">
  </a>
  <a href="https://huggingface.co/spaces/lniki/track-anything-annotate">
    <img src="https://img.shields.io/badge/🤗-Hugging_Face_Space-FFD21E.svg?style=flat-square&logo=huggingface&logoColor=black" alt="Open in Spaces">
  </a>
  <a href="https://github.com/lnikioffic/track-anything-annotate/blob/main/LICENSE">
    <img src="https://img.shields.io/badge/License-MIT-green.svg?style=flat-square" alt="License">
  </a>

  <p align="center">
    <strong>Инструмент полуавтоматической генерации датасетов на базе SAM 2 и XMem++</strong>
    <br />
    <i>«Не размечайте кадр за кадром — отслеживайте и сегментируйте одним кликом»</i>
  </p>

</div>

---

## 📖 О проекте

**Track Anything Annotate** — это инструмент нового поколения для быстрой подготовки датасетов на основе видео. Мы решаем главную проблему CV-инженеров и разметчиков: **ручная аннотация видео отнимает часы и вызывает выгорание**.

Вместо монотонной обводки полигонов на каждом кадре, вы используете мощь **Segment Anything Model 2 (SAM 2)** для инициализации и **XMem++** для устойчивого трекинга. Это позволяет ускорить процесс создания датасета в **3-5 раз**.

### 🔥 Почему это нужно?
*   **⏱️ Экономия времени:** то, что раньше занимало часы, теперь делается за минуты.
*   **🎯 Точность SOTA уровня:** SAM 2 генерирует идеальные маски, а XMem++ удерживает объект даже при перекрытиях.
*   **🧠 Human-in-the-Loop:** вы не теряете контроль. Вы можете разметить видео по фрагментам, если объекты не находятся в кадре в течение всего видео.
*   **📦 Готовый результат:** на выходе вы получаете структуру папок с картинками и текстовыми файлами **YOLO** или **COCO**, готовыми для обучения моделей (YOLOv8, YOLO11 и др.).

---

## 🖼️ Демонстрация работы

<div align="center">
  <img src="video-test/cache/image.png" width="80%" alt="Скриншот интерфейса">
</div>

---

## 🛠️ Установка (Запуск в 3 шага)

Мы сделали установку максимально простой, так как понимаем, что никто не хочет возиться с зависимостями часами.

### Вариант 1: Через `uv` (Рекомендуется, быстро и надежно)

```bash
# 1. Установите зависимости (выберите команду под ваше железо)
uv sync --extra cu129  # Для NVIDIA GPU (CUDA 12)
# uv sync --extra cpu  # Для процессора (медленнее)

# 2. Скачайте веса нейросетей
uv run checkpoints/download_models.py

# 3. Запустите интерфейс
uv run gradio demo.py
```

### Вариант 2: Через стандартный `pip`

<details>
<summary>Нажмите, чтобы развернуть инструкцию для pip</summary>

#### Windows / Linux
```bash
# Клонирование репозитория
git clone https://github.com/lnikioffic/track-anything-annotate.git
cd track-anything-annotate

# Установка зависимостей (пример для CUDA 12.x)
pip install -r requirements.txt --index-url https://download.pytorch.org/whl/cu129

# Загрузка моделей
python checkpoints/download_models.py

# Запуск
python demo.py
```
</details>

> Демо-нтерфейс откроется по адресу: **http://127.0.0.1:8080**

---

## 🚀 Как пользоваться

1.  **Загрузка видео:** Перетащите файл в окно браузера.
2.  **Выбор объекта:**
    *   Поставьте **точку** (левый клик) на объекте.
3.  **Трекинг:** Нажмите кнопку `Track`. Система обработает видео и отобразит пробный результат разметки.

### CLI режим (для пакетной обработки)
Если вам нужно обработать видео без интерфейса:

```bash
uv run annotation.py --video-path video.mp4 --names-class cat --type-save yolo
```

---

## 📊 Производительность

Мы сравнили наш подход с прошлой версией нашего проекта (FastSAM + OpenCV Tracker). Результаты подтверждают, что использование тяжелых моделей оправдано качеством.

| Метод | Инициализация (мс) | Скорость (мс/кадр) | VRAM (МБ) | Качество (IoU) |
| :--- | :---: | :---: | :---: | :---: |
| **FastSAM + OpenCV** | 1357 | 15 | ~607 | Низкое |
| **SAM 2 + XMem++ (Наш)** | 2722 | 50 | ~1476 | **Высокое (+20%)** |

---

## 🗺️ Roadmap и Планы

Основываясь на обратной связи от пользователей (CustDev), мы планируем следующие улучшения:

*   [x] Трекинг одного объекта и экспорт в YOLO.
*   [x] **Новые форматы экспорта:** Добавление поддержки COCO JSON (частый запрос от исследователей).
*   [ ] **Новые форматы экспорта:** Добавление поддержки Pascal VOC XML (частый запрос от исследователей).      
*   [ ] **Мульти-классовая разметка:** Возможность отслеживать несколько разных объектов одновременно.
*   [ ] **Разметка изображений:** Возможность собрать и разметить свой датасет на основе изображений.

---

## 📚 Цитирование

Если вы используете этот инструмент в научной работе, пожалуйста, сошлитесь на нашу статью:

```bibtex
@article{ivanov2025track,
    title={Track Anything Annotate: Video annotation and dataset generation of computer vision models},
    author={Ivanov, Nikita and Klimov, Mark and Glukhikh, Dmitry and Chernysheva, Tatiana and Glukhikh, Igor},
    journal={arXiv preprint arXiv:2505.17884},
    year={2025}
}
```

---

<div align="center">
    Created by <a href="https://github.com/lnikioffic">lnikioffic</a> | Тюменский государственный университет
</div>
