# YouTube Video to MP3 Converter

Este é um script em Python que baixa vídeos do YouTube e os converte para arquivos MP3. Utiliza a biblioteca `pytube` para baixar os vídeos e `moviepy` para a conversão.

## Funcionalidades

- **Baixar Vídeos do YouTube:** Baixa vídeos em alta resolução a partir de URLs fornecidas.
- **Converter Vídeos para MP3:** Converte os vídeos baixados em arquivos de áudio MP3.
- **Remover Vídeos Após Conversão:** Remove o arquivo de vídeo original após a conversão para economizar espaço.

## Tecnologias Utilizadas

- Python
- pytube
- moviepy

## Pré-requisitos

- Python 3.x
- Bibliotecas Python:
  - `pytube`
  - `moviepy`

## Instalação

1. Clone o repositório:
    ```bash
    git clone https://github.com/seu-usuario/youtube-to-mp3-converter.git
    cd youtube-to-mp3-converter
    ```

2. Instale as dependências:
    ```bash
    pip install pytube moviepy
    ```

## Uso

1. Execute o script:
    ```bash
    python youtube_to_mp3.py
    ```

2. Insira a URL do vídeo do YouTube quando solicitado:
    ```plaintext
    Digite a URL do vídeo do YouTube:
    ```

3. O vídeo será baixado e convertido para MP3 no diretório padrão de Downloads do usuário:
    ```plaintext
    Download concluído e vídeo convertido para MP3: /caminho/para/Downloads/video.mp3
    ```

## Código Exemplo

```python
import os
import pytube
from moviepy.editor import *
import ssl

# Configura o contexto SSL para ignorar verificações de certificado
ssl._create_default_https_context = ssl._create_unverified_context

def download_and_convert_to_mp3(url, output_folder):
    try:
        # Baixa o vídeo do YouTube
        youtube = pytube.YouTube(url)
        video = youtube.streams.get_highest_resolution()
        video_path = video.download(output_folder)
        
        # Converte o vídeo para MP3
        file_name = os.path.splitext(os.path.basename(video_path))[0]
        mp3_path = os.path.join(output_folder, f"{file_name}.mp3")
        
        video_clip = VideoFileClip(video_path)
        video_clip.audio.write_audiofile(mp3_path)
        video_clip.close()
        
        # Remove o arquivo de vídeo após a conversão
        os.remove(video_path)
        
        return mp3_path
    except Exception as e:
        print(f"Erro ao processar o vídeo: {str(e)}")
        return None

if __name__ == "__main__":
    url = input("Digite a URL do vídeo do YouTube: ")
    output_folder = os.path.expanduser("~/Downloads")  # Diretório padrão de Downloads
    
    mp3_path = download_and_convert_to_mp3(url, output_folder)
    
    if (mp3_path):
        print(f"Download concluído e vídeo convertido para MP3: {mp3_path}")
    else:
        print("Falha ao processar o vídeo.")
