# Windows Administration Guide
1. Introduction
- Tujuan dokumen: Panduan administrasi Windows untuk menjaga sistem berjalan optimal dan aman.
- Target pembaca: Administrator sistem, teknisi IT.

2. User and Group Management
- Membuat, mengubah, dan menghapus user
Contoh:

```powershell
New-LocalUser -Name "UserTest" -Password (ConvertTo-SecureString "P@ssw0rd" -AsPlainText -Force)
Add-LocalGroupMember -Group "Administrators" -Member "UserTest"
```
- Mengelola hak akses dan izin folder/file
Contoh: Setting izin menggunakan GUI atau perintah icacls.

3. Disk and Storage Management
- Mengelola partisi dan volume
Contoh:

```powershell
Get-Partition
Resize-Partition -DriveLetter C -Size 100GB
```
- Menggunakan fitur Unified Write Filter (UWF)
UWF adalah fitur Windows yang melindungi drive dengan mengalihkan semua perubahan penulisan ke overlay sementara, sehingga saat reboot sistem kembali ke kondisi awal. Cocok untuk perangkat embedded atau sistem kiosk.

Menggunakan uwfmgr untuk mengelola UWF
```powershell
a. Cek status UWF (Menampilkan konfigurasi dan status UWF): uwfmgr get-config
b. Aktifkan filter pada drive C: uwfmgr filter enable
c. Nonaktifkan filter: uwfmgr filter disable
d. Menambahkan pengecualian folder/file agar tidak tertutup UWF: uwfmgr volume set-exclusion C:\Data /add
Memastikan folder C:\Data tetap bisa diubah secara permanen.
e. Menghapus pengecualian: uwfmgr volume set-exclusion C:\Data /remove
```
Reboot diperlukan setelah mengaktifkan atau menonaktifkan filter agar perubahan berlaku.

Contoh penggunaan
Aktifkan UWF untuk drive sistem:
```powershell
uwfmgr filter enable
```
Tambahkan folder pengecualian agar data di D:\Logs tetap tersimpan:
```powershell
uwfmgr volume set-exclusion D:\Logs /add
```
Reboot sistem:
```powershell
shutdown /r /t 0
```

```powershell
uwfmgr filter enable
uwfmgr get-config
```

4. Task Scheduling
- Membuat dan mengelola Scheduled Tasks
Contoh:
```powershell
schtasks /Create /TN "BackupTask" /TR "backup.ps1" /SC DAILY /ST 06:00 /RL HIGHEST
```
- Penjelasan opsi penting seperti: menjalankan task walau user tidak login, run with highest privileges.

5. Service Management
- Memulai, menghentikan, dan mengatur service
Contoh:
```powershell
Get-Service -Name "Spooler"
Stop-Service -Name "Spooler"
Start-Service -Name "Spooler"
Set-Service -Name "Spooler" -StartupType Automatic
```

6. Security and Updates
- Mengatur Windows Defender
- Konfigurasi Windows Update otomatis
- Manajemen firewall
Contoh:
```powershell
New-NetFirewallRule -DisplayName "Allow HTTP" -Direction Inbound -LocalPort 80 -Protocol TCP -Action Allow
```

7. Backup and Recovery
- Membuat backup dengan Task Scheduler dan PowerShell
- Restore data dan konfigurasi
Contoh: Panduan singkat membuat task backup otomatis.

8. Monitoring and Logging
- Melihat Event Logs
Contoh:
```powershell
Get-EventLog -LogName System -Newest 50
```
- Memantau kinerja sistem

9. Troubleshooting
- Cara mendiagnosa masalah umum
- Reset konfigurasi yang bermasalah
- Contoh: Reset network adapter, flush DNS cache.
