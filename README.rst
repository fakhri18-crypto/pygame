import pygame
import random
import sys

# Inisialisasi Pygame
pygame.init()

# Warna
PUTIH = (255, 255, 255)
HITAM = (0, 0, 0)
MERAH = (213, 50, 80)
HIJAU = (0, 255, 0)
BIRU = (50, 153, 213)

# Ukuran layar
LEBAR = 600
TINGGI = 600
UKURAN_KOTAK = 20

# Buat layar
layar = pygame.display.set_mode((LEBAR, TINGGI))
pygame.display.set_caption('Game Ular - Snake Game')
jam = pygame.time.Clock()

class Ular:
    def __init__(self):
        self.panjang = 1
        self.posisi = [(LEBAR//2, TINGGI//2)]
        self.arah = (0, -UKURAN_KOTAK)  # Mulai ke atas
        self.tumbuh = False
        
    def gerak(self):
        kepala = self.posisi[0]
        x, y = kepala
        dx, dy = self.arah
        
        # Pindah kepala ke posisi baru
        kepala_baru = (x + dx, y + dy)
        self.posisi.insert(0, kepala_baru)
        
        # Jika tidak tumbuh, hapus ekor
        if not self.tumbuh:
            self.posisi.pop()
        else:
            self.tumbuh = False
            
    def ubah_arah(self, arah_baru):
        # Cegah berbalik arah
        if (arah_baru[0] * -1, arah_baru[1] * -1) != self.arah:
            self.arah = arah_baru
            
    def cek_tabrakan(self):
        kepala = self.posisi[0]
        # Tabrakan dinding
        if (kepala[0] <= 0 or kepala[0] >= LEBAR or 
            kepala[1] <= 0 or kepala[1] >= TINGGI):
            return True
            
        # Tabrakan diri sendiri
        if kepala in self.posisi[1:]:
            return True
            
        return False
        
    def gambar(self, layar):
        for pos in self.posisi:
            pygame.draw.rect(layar, HIJAU, 
                           (pos[0], pos[1], UKURAN_KOTAK, UKURAN_KOTAK))
            pygame.draw.rect(layar, HITAM, 
                           (pos[0], pos[1], UKURAN_KOTAK, UKURAN_KOTAK), 1)

class Makanan:
    def __init__(self):
        self.posisi = self.posisi_acak()
        
    def posisi_acak(self):
        x = random.randint(0, (LEBAR-UKURAN_KOTAK)//UKURAN_KOTAK) * UKURAN_KOTAK
        y = random.randint(0, (TINGGI-UKURAN_KOTAK)//UKURAN_KOTAK) * UKURAN_KOTAK
        return (x, y)
        
    def gambar(self, layar):
        pygame.draw.rect(layar, MERAH, 
                        (self.posisi[0], self.posisi[1], 
                         UKURAN_KOTAK, UKURAN_KOTAK))

def tampilkan_skor(layar, skor):
    font = pygame.font.SysFont(None, 35
