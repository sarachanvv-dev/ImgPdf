# img-pdf-gui

Графічна обгортка (PyQt6) та консольний інструмент для автоматичного об'єднання зображень (JPG, PNG тощо) у єдиний PDF-документ з оптимізацією розміру через Ghostscript та записом метаданих через ExifTool.

---

## Структура файлів та куди їх встановлювати

Для коректної роботи системи у вашому домашньому каталозі (`~`) мають бути розміщені наступні файли:

| Шлях встановлення                                                                                                   | Файл                         | Опис та призначення                                                                                                                |
| :------------------------------------------------------------------------------------------------------------------ | :--------------------------- | :--------------------------------------------------------------------------------------------------------------------------------- |
| `~/.local/bin/img-pdf`                                                                                              | **`img-pdf`**                | Головний консольний Bash-скрипт, який виконує збирання (`img2pdf`), стиснення (`ghostscript`) та додавання метаданих (`exiftool`). |
| `~/.local/bin/img-pdf-gui`                                                                                          | **`img-pdf-gui`**            | Графічний інтерфейс на чистому Python (PyQt6) для вибору параметрів, шляху збереження та перегляду логів у реальному часі.         |
| `~/.local/share/kio/servicemenus/img-pdf-action.desktop`<br>*(або `~/.local/share/kxmlgui5/servicemenus/` у KDE 6)* | **`img-pdf-action.desktop`** | Контекстне меню для файлового менеджера Dolphin, що дозволяє викликати конвертацію для виділених файлів.                           |

---

## Передумови (Залежності)

Переконайтеся, що у вашій системі встановлено необхідні пакети (приклад для Arch Linux / Manjaro / Fedora / Ubuntu):

```bash
# Arch Linux / Manjaro
sudo pacman -S python-pyqt6 img2pdf ghostscript perl-image-exiftool

# Ubuntu / Debian
sudo apt install python3-pyqt6 img2pdf ghostscript libimage-exiftool-perl
```

## Покрокова інструкція зі встановлення

### Крок 1. Створення необхідних директорій

```Bash
mkdir -p ~/.local/bin
mkdir -p ~/.local/share/kio/servicemenus
```

### Крок 2. Розміщення та надання прав на виконання

1. Збережіть скрипт обробки у `~/.local/bin/img-pdf`.

2. Збережіть Python-скрипт GUI у `~/.local/bin/img-pdf-gui`.

3. Надайте обом файлам права на виконання:

    ```bash
    chmod +x ~/.local/bin/img-pdf
    chmod +x ~/.local/bin/img-pdf-gui
    ```

4. Переконайтеся, що каталог `~/.local/bin` додано до вашої змінної `PATH` (зазвичай у `~/.bashrc` або `~/.zshrc`):

    ```bash
    export PATH="$HOME/.local/bin:$PATH"
    ```

### Крок 3. Налаштування контекстного меню Dolphin (KDE Plasma)

Створіть файл `~/.local/share/kio/servicemenus/img-pdf-action.desktop` із наступним вмістом:

```Ini
[Desktop Entry]
Type=Service
MimeType=image/jpeg;image/png;image/tiff;image/webp;image/x-adobe-dng;
Actions=convertToPdf;
X-KDE-Native-Submenu=Image

[Desktop Action convertToPdf]
Name=Зібрати в PDF (GUI)
Icon=application-pdf
Exec=img-pdf-gui %F
```

## Як користуватися

### Варіант 1. Через контекстне меню Dolphin (KDE)

1. Виділіть одне або кілька зображень у файловому менеджері Dolphin.

2. Натисніть правою кнопкою миші ➔ Конвертація ➔ Зібрати в PDF (GUI).

3. У вікні, що відкриється, виберіть рівень якості стиснення та введіть метадані.

4. Вкажіть шлях збереження PDF-файлу.

5. Відслідковуйте процес у графічному вікні термінала.

### Варіант 2. Через термінал

Запуск графічного інтерфейсу з передачею файлів:

```Bash
img-pdf-gui image1.jpg image2.png
```

Запуск напряму через консольний скрипт (без GUI):

```Bash
img-pdf -q ebook -o ~/output.pdf -a "Автор" -t "Назва" image1.jpg image2.png
```

## Ліцензія

Цей проєкт поширюється під ліцензією **MIT.**

Copyright © 2026 Hotabich.
