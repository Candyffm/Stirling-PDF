# OCR Language Packs and Setup

This document provides instructions on how to add additional language packs for the OCR tab in Stirling-PDF, both inside and outside of Docker.

## How does the OCR Work
Stirling-PDF uses OCRmyPDF which in turn uses tesseract for its text recognition.
All credit goes to them for this awesome work! 

## Language Packs

Tesseract OCR supports a variety of languages. You can find additional language packs in the Tesseract GitHub repositories:

- [tessdata_fast](https://github.com/tesseract-ocr/tessdata_fast): These language packs are smaller and faster to load, but may provide lower recognition accuracy.
- [tessdata](https://github.com/tesseract-ocr/tessdata): These language packs are larger and provide better recognition accuracy, but may take longer to load.

Depending on your requirements, you can choose the appropriate language pack for your use case. By default Stirling-PDF uses the tessdata_fast eng but this can be replaced.

### Installing Language Packs

1. Download the desired language pack(s) by selecting the `.traineddata` file(s) for the language(s) you need.
2. Place the `.traineddata` files in the Tesseract tessdata directory: `/usr/share/tesseract-ocr/4.00/tessdata` (Debian) or `/usr/share/tesseract/tessdata` (Fedora)

# DO NOT REMOVE EXISTING ENG.TRAINEDDATA, ITS REQUIRED.

#### Docker

If you are using Docker, you need to expose the Tesseract tessdata directory as a volume in order to use the additional language packs. 
#### Docker Compose
Modify your `docker-compose.yml` file to include the following volume configuration:


```yaml
services:
  your_service_name:
    image: your_docker_image_name
    volumes:
      - /location/of/trainingData:/usr/share/tesseract-ocr/4.00/tessdata
```


#### Docker run
Add the following to your existing docker run command
```bash
-v /location/of/trainingData:/usr/share/tesseract-ocr/4.00/tessdata
```

#### Non-Docker
If you are not using Docker, you need to install the OCR components, including the ocrmypdf app.
You can see [OCRmyPDF install guide](https://ocrmypdf.readthedocs.io/en/latest/installation.html)

Debian based systems, install languages with this command:

```bash
sudo apt update &&\
# All languages
# sudo apt install -y 'tesseract-ocr-*'

# Find languages:
apt search tesseract-ocr-

# View installed languages:
dpkg-query -W tesseract-ocr- | sed 's/tesseract-ocr-//g'
```

Fedora:

```bash
# All languages
# sudo dnf install -y tesseract-langpack-*

# Find languages:
dnf search -C tesseract-langpack-

# View installed languages:
rpm -qa | grep tesseract-langpack | sed 's/tesseract-langpack-//g'
```
