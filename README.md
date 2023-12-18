import cv2
from pyzbar.pyzbar import decode
import qrcode
import markdown
import requests
from io import BytesIO

def scan_qr_code(image_path):
    # ... (same as before)

def generate_qr_code(data, output_path='output_qr_code.png'):
    # ... (same as before)

def scan_qr_codes_from_readme(readme_url):
    # Download the README.md file
    response = requests.get(readme_url)
    readme_content = response.text

    # Parse the markdown file
    html_content = markdown.markdown(readme_content)

    # Find all image URLs in the README.md file
    image_urls = []
    for token in markdown.util.tokenizer(html_content):
        if token[0] == 'image':
            image_urls.append(token[1]['src'])

    # Scan QR codes from each image URL
    for image_url in image_urls:
        response = requests.get(image_url)
        image = cv2.imdecode(np.frombuffer(response.content, np.uint8), -1)
        scan_qr_code(image)

if __name__ == "__main__":
    # Example: Scan QR codes from images mentioned in README.md
    scan_qr_codes_from_readme("https://raw.githubusercontent.com/username/repo/main/README.md")
