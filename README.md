# Ahmed AKUL, Ahmed HAMIDI ve ELMUNZİR ELSEYER *Görüntülerden Renk Paleti Oluşturma* SAYISAL GÖRÜNTÜ IŞLEME projesi 
# DIP
from PIL import Image
import colorsys
import matplotlib.pyplot as plt
import numpy as np

# Resmi açın
image_path = r" " # Buraya kendi resminizin yolunu yazın
image = Image.open(image_path)

# Resmi RGB formatında piksel verileri olarak al
rgb_data = image.convert('RGB').getdata()

# Benzersiz RGB renklerini elde et
unique_rgb = list(set(rgb_data))

# Sadece ilk 10 benzersiz rengi seç
unique_rgb = unique_rgb[:10]

# RGB'den HSV'ye dönüşüm
hsv_list = []
for rgb in unique_rgb:
    r, g, b = rgb
    # RGB değerlerini [0, 1] aralığına dönüştür
    r, g, b = r / 255.0, g / 255.0, b / 255.0
    h, s, v = colorsys.rgb_to_hsv(r, g, b)
    # HSV'yi [0, 360] (hue) ve [0, 1] (saturation, value) aralığına dönüştür
    hsv_list.append((h * 360, s * 100, v * 100))

# RGB ve HSV paletlerini oluşturmak için renkleri NumPy dizisine dönüştürme
rgb_colors = np.array(unique_rgb) / 255.0  # RGB renklerini [0, 1] aralığına dönüştür
hsv_colors = np.array(hsv_list)[:, 0]  # Sadece hue değerleri kullanılacak
hsv_colors = plt.cm.hsv(hsv_colors / 360.0)  # HSV değerlerini matplotlib renk haritasına dönüştür

# RGB ve HSV paletlerini yan yana göstermek
fig, (ax1, ax2) = plt.subplots(1, 2, figsize=(16, 4))

# RGB paleti
ax1.imshow([rgb_colors], aspect='auto')
ax1.axis('off')
ax1.set_title("RGB Color Palette")

# HSV paleti
ax2.imshow([hsv_colors], aspect='auto')
ax2.axis('off')
ax2.set_title("HSV Color Palette")

plt.show()

# Benzersiz RGB ve HSV değerlerini yazdırmak (ilk 10 renk örneği)
for i in range(len(unique_rgb)):
    print(f"RGB: {unique_rgb[i]} -> HSV: {hsv_list[i]}")
