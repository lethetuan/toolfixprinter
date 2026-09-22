Trong môi trường doanh nghiệp hoặc gia đình dùng nhiều máy tính, dùng chung máy in qua mạng là cách tối ưu để tiết kiệm nguồn lực và nâng cao hiệu suất. Tuy nhiên, tình trạng không thể chia sẻ máy in vẫn xuất hiện phổ biến trên Windows 11, 10 hay các bản cũ hơn, dù thiết bị đã nhận đủ driver và cùng kết nối vào một hệ thống modem wifi hoặc switch mạng chung.


## Cách Nhận Biết Máy In Đang Bị Lỗi Chia Sẻ (Win 10 & Win 11)

# Hướng Dẫn Tạo Và Sử Dụng Tool Fix Lỗi Máy In LAN & USB 
**Nguồn/Tác giả:** lethetuanpc.blogspot.com (Batch Version)

---

## 1. Giới thiệu công cụ

Đây là một đoạn mã script (Batch file) chạy trên nền CMD của Windows, được thiết kế để tự động khắc phục hầu hết các lỗi máy in qua mạng LAN và USB phổ biến nhất hiện nay (đặc biệt là sau các bản cập nhật bảo mật của Windows).

**Các tính năng nổi bật:**
*   **Dành cho máy chủ (Máy cắm trực tiếp máy in):** Khắc phục lỗi chia sẻ (PrintNightmare 0x0000011b, 0x0000007c, 0x000006d9), tự động chia sẻ tất cả máy in.
*   **Dành cho máy khách (Máy kết nối qua mạng):** Sửa lỗi không tìm thấy máy in, không kết nối được (0x00000bc4, 0x00000709, Access Denied 0x00004005, v.v.).
*   **Sửa lỗi máy in USB (Đặc biệt dòng Canon LBP 2900/3300):** Fix lỗi Communication Error, reset USB Monitor, tự động xóa kẹt lệnh in (Spooler).
*   **Công cụ tích hợp:** Quản lý thông tin đăng nhập (Credential), khởi động lại tiến trình in, bật tường lửa đúng cách và tính năng **Auto Fix 15 bước** quét và sửa lỗi toàn diện.

---

## 2. Cách tạo file công cụ trên máy tính

Bạn không cần cài đặt phần mềm bên thứ 3, chỉ cần làm theo các bước sau để tạo file chạy trực tiếp:

**Bước 1:** Mở ứng dụng **Notepad** trên Windows (Click chuột phải chọn `New` -> `Text Document` và mở lên).

<img width="490" height="392" alt="image" src="https://github.com/user-attachments/assets/b6bc65a5-af03-466a-9159-ec2dd1819a03" />

<img width="602" height="429" alt="image" src="https://github.com/user-attachments/assets/4a1d5f5e-9a2a-4eea-a18e-c946fb69b895" />

**Bước 2:** Copy toàn bộ đoạn mã Batch dưới đây và dán vào Notepad:

```bat
@echo off
chcp 65001 >nul
title TOOL FIX MAY IN LAN ^& USB - LETHETUANPC.BLOGSPOT.COM (BATCH VERSION)
color 0B

:: ==========================================
:: YEU CAU QUYEN ADMINISTRATOR
:: ==========================================
net session >nul 2>&1
if %errorLevel% == 0 (
    goto :MainMenu
) else (
    echo [!] Vui long chay tool voi quyen Administrator! (Run as administrator)
    echo Dang thu tu dong cap quyen...
    powershell -Command "Start-Process -FilePath '%0' -Verb RunAs"
    exit
)

:MainMenu
cls
echo ==============================================================================
echo                      SUA LOI MAY IN LAN ^& USB - LETHETUANPC.BLOGSPOT.COM
echo ==============================================================================
echo.
echo  [SERVER - MAY CHIA SE]                      [CLIENT - MAY KET NOI]
echo  1. Fix Connect Printer                      10. Fix 0x00000bc4
echo  2. Fix 0x0000011b (PrintNightmare)          11. Fix 0x00000709 (Set Default)
echo  3. Fix 0x0000007c                           12. Fix Cannot Connect
echo  4. Fix 0x000006d9                           13. Fix Policy Connect (GP Block)
echo  5. Auto Share Tat Ca May In                 14. Fix 0x00004005 (Access Denied)
echo                                              15. Fix 0x000003e3
echo  [CA 2 MAY]                                  16. Fix 0x00000bcb
echo  6. Fix 0x00000040 (Firewall Block)          17. Fix 0x0000007e (Driver)
echo  7. Fix 0x000006ba (RPC)                     18. Fix 0x00000012 (Spool Error)
echo  8. Fix Comm Error (Canon LBP 2900/3300)     19. Fix 0x000003eb
echo                                              20. Fix 0x00000771 (Offline)
echo  [CONG CU BO SUNG]
echo  21. Them Credential        25. Xoa Hang Doi In       29. Set LocalConnection
echo  22. Xem Credential         26. Reset USB Monitor     30. Fix Canon 2900/3300
echo  23. Xoa Credential         27. Fix Print Spooler Svc 31. Unlock Share Printer
echo  24. Restart Spooler        28. Reset PrinterPorts    32. Xoa Driver Canon
echo.
echo  33. [★ AUTO FIX 15 BUOC] Chay tu dong toan dien loi mang
echo  34. Mo Device/Print Management
echo  0. Thoat
echo ==============================================================================
set /p opt="Chon chuc nang (0-34): "

if "%opt%"=="1" goto :FixConnectServer
if "%opt%"=="2" goto :Fix11b
if "%opt%"=="3" goto :Fix7c
if "%opt%"=="4" goto :Fix6d9
if "%opt%"=="5" goto :AutoShare
if "%opt%"=="6" goto :Fix40
if "%opt%"=="7" goto :Fix6ba
if "%opt%"=="8" goto :FixCommError
if "%opt%"=="10" goto :Fixbc4
if "%opt%"=="11" goto :Fix709
if "%opt%"=="12" goto :FixCannotConnect
if "%opt%"=="13" goto :FixPolicy
if "%opt%"=="14" goto :Fix4005
if "%opt%"=="15" goto :Fix3e3
if "%opt%"=="16" goto :Fixbcb
if "%opt%"=="17" goto :Fix7e
if "%opt%"=="18" goto :Fix12
if "%opt%"=="19" goto :Fix3eb
if "%opt%"=="20" goto :Fix771
if "%opt%"=="21" goto :AddCred
if "%opt%"=="22" goto :ViewCred
if "%opt%"=="23" goto :DelCred
if "%opt%"=="24" goto :RestartSpoolerOnly
if "%opt%"=="25" goto :ClearSpool
if "%opt%"=="26" goto :ResetUSBMon
if "%opt%"=="27" goto :FixSpoolerSvc
if "%opt%"=="28" goto :ResetPorts
if "%opt%"=="29" goto :SetLocalConn
if "%opt%"=="30" goto :FixCanon2900
if "%opt%"=="31" goto :UnlockShare
if "%opt%"=="32" goto :DelCanonDriver
if "%opt%"=="33" goto :AutoFix15
if "%opt%"=="34" goto :OpenMgmt
if "%opt%"=="0" exit
goto :MainMenu

:: ==========================================
:: SUBROUTINES DUNG CHUNG
:: ==========================================
:OpenFirewall
netsh advfirewall firewall set rule group="File and Printer Sharing" new enable=Yes >nul
netsh advfirewall firewall set rule group="Network Discovery" new enable=Yes >nul
exit /b

:SetRPC
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Print" /v RpcAuthnLevelPrivacyEnabled /t REG_DWORD /d 0 /f >nul
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC" /v RpcOverNamedPipes /t REG_DWORD /d 1 /f >nul
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\RPC" /v RpcOverTcp /t REG_DWORD /d 1 /f >nul
exit /b

:CopyMSCMS
if exist "%SystemRoot%\System32\mscms.dll" if exist "%SystemRoot%\System32\spool\drivers\x64\3" if not exist "%SystemRoot%\System32\spool\drivers\x64\3\mscms.dll" copy /y "%SystemRoot%\System32\mscms.dll" "%SystemRoot%\System32\spool\drivers\x64\3\mscms.dll" >nul
if exist "%SystemRoot%\System32\mscms.dll" if exist "%SystemRoot%\System32\spool\drivers\w32x86\3" if not exist "%SystemRoot%\System32\spool\drivers\w32x86\3\mscms.dll" copy /y "%SystemRoot%\System32\mscms.dll" "%SystemRoot%\System32\spool\drivers\w32x86\3\mscms.dll" >nul
exit /b

:RestartSpooler
echo [INFO] Dung Print Spooler...
sc stop spooler >nul 2>&1
taskkill /f /im spoolsv.exe >nul 2>&1
timeout /t 2 >nul
echo [INFO] Khoi dong lai Print Spooler...
sc start spooler >nul 2>&1
echo [OK] Spooler khoi dong thanh cong!
exit /b

:DeleteCNBJNP
echo [INFO] Dang kiem tra va xoa cong CNBJNP...
powershell -Command "$p='HKLM:\SYSTEM\CurrentControlSet\Control\Print\Monitors'; Get-ChildItem -Path $p \vert{} Where-Object {$_.Name -match 'CNBJNP'} | ForEach-Object { Remove-Item -Path $_.PSPath -Recurse -Force; Write-Host 'Da xoa:'$_.Name }"
exit /b

:: ==========================================
:: CAC CHUC NANG CHINH
:: ==========================================

:FixConnectServer
echo --- Fix Connect Printer (Server) ---
call :OpenFirewall
call :SetRPC
call :CopyMSCMS
sc config Spooler start= auto >nul
sc config fdPHost start= auto >nul
sc config FDResPub start= auto >nul
sc config SSDPSRV start= auto >nul
sc config upnphost start= auto >nul
sc start fdPHost >nul & sc start FDResPub >nul & sc start SSDPSRV >nul & sc start upnphost >nul
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:Fix11b
echo --- Fix 0x0000011b ---
call :SetRPC
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:Fix7c
echo --- Fix 0x0000007c ---
call :SetRPC
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:Fix6d9
echo --- Fix 0x000006d9 ---
sc config MpsSvc start= auto >nul
sc start MpsSvc >nul
call :OpenFirewall
echo [OK] Windows Firewall da duoc bat va cau hinh.
pause & goto MainMenu

:AutoShare
echo --- Auto Share Tat Ca May In ---
echo Dang xu ly chia se, vui long cho...
set "psf=%temp%\autoshare.ps1"

> "%psf%" echo $printers = Get-WmiObject -Class Win32_Printer -Filter 'Network=False'
>> "%psf%" echo $printers ^| ForEach-Object {
>> "%psf%" echo     $name =$_.Name
>> "%psf%" echo     $sn = ($name -replace '[^^a-zA-Z0-9\-_]','')
>> "%psf%" echo     if ($sn.Length -gt 12) { $sn =$sn.Substring(0,12) }
>> "%psf%" echo     if (-not $sn) {$sn = 'Printer' }
>> "%psf%" echo     $argList = "printui.dll,PrintUIEntry /Xs /n `"$name`" Shared TRUE ShareName `"$sn`""
>> "%psf%" echo     Start-Process -FilePath 'rundll32.exe' -ArgumentList $argList -Wait -NoNewWindow
>> "%psf%" echo     Write-Host "Da share: $name --^>$sn"
>> "%psf%" echo }

powershell -NoProfile -ExecutionPolicy Bypass -File "%psf%"
del /q "%psf%" >nul 2>&1

echo [OK] Hoan tat!
pause & goto MainMenu

:Fix40
echo --- Fix 0x00000040 ---
call :OpenFirewall
echo [OK] Da mo Firewall cho File and Printer Sharing.
pause & goto MainMenu

:Fix6ba
echo --- Fix 0x000006ba ---
sc config RpcSs start= auto >nul
sc start RpcSs >nul
sc config RpcEptMapper start= auto >nul
sc start RpcEptMapper >nul
call :SetRPC
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:FixCommError
echo --- Fix Communication Error (Canon LBP) ---
net stop spooler >nul 2>&1
taskkill /f /im spoolsv.exe >nul 2>&1
del /q /f "%SystemRoot%\System32\spool\PRINTERS\*.*" >nul 2>&1
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Print\Monitors\USB Monitor" /f >nul 2>&1
call :DeleteCNBJNP
call :RestartSpooler
echo [ACTION] Vui long RUT CAP USB, sau do CAM LAI de thu in!
pause & goto MainMenu

:Fixbc4
echo --- Fix 0x00000bc4 ---
call :SetRPC
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:Fix709
echo --- Fix 0x00000709 ---
net stop spooler >nul 2>&1
dism /Online /Enable-Feature /FeatureName:Printing-Foundation-InternetPrinting-Client /NoRestart
dism /Online /Enable-Feature /FeatureName:Printing-LPRPortMonitor /NoRestart
echo Phan quyen FullControl cho User hien tai tren Registry...
powershell -Command "$p='HKCU:\Software\Microsoft\Windows NT\CurrentVersion\Windows';$acl=Get-Acl $p; $rule=New-Object System.Security.AccessControl.RegistryAccessRule('Everyone','FullControl','ContainerInherit,ObjectInherit','None','Allow'); $acl.SetAccessRule($rule); Set-Acl -Path $p -AclObject$acl; Write-Host 'Phan quyen OK.'"
reg delete "HKCU\Software\Microsoft\Windows NT\CurrentVersion\Windows" /v Device /f >nul 2>&1
reg add "HKCU\Software\Microsoft\Windows NT\CurrentVersion\Windows" /v LegacyDefaultPrinterMode /t REG_DWORD /d 1 /f >nul
call :RestartSpooler
echo [ACTION] Vao Control Panel > Xoa may in bi loi > Add lai > Set Default.
pause & goto MainMenu

:FixCannotConnect
echo --- Fix Cannot Connect to Printer ---
sc config spooler start= auto >nul
sc start spooler >nul
call :OpenFirewall
call :CopyMSCMS
call :RestartSpooler
echo [OK] Thu ket noi lai voi cu phap: \\IP_Server\TenMayIn
pause & goto MainMenu

:FixPolicy
echo --- Fix Policy Connect (GP Block) ---
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v RestrictDriverInstallationToAdministrators /t REG_DWORD /d 0 /f >nul
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v NoWarningNoElevationOnInstall /t REG_DWORD /d 1 /f >nul
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v UpdatePromptSettings /t REG_DWORD /d 2 /f >nul
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v InForest /t REG_DWORD /d 0 /f >nul
call :RestartSpooler
echo [OK] Da go chan Policy.
pause & goto MainMenu

:Fix4005
echo --- Fix 0x00004005 ---
cmdkey /list
echo [INFO] Ban can them Credential neu chua co thong tin luu tru.
call :SetRPC
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:Fix3e3
echo --- Fix 0x000003e3 ---
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v forceguest /t REG_DWORD /d 0 /f >nul
call :SetRPC
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:Fixbcb
echo --- Fix 0x00000bcb ---
sc config LanmanWorkstation start= auto >nul
sc start LanmanWorkstation >nul
sc config LanmanServer start= auto >nul
sc start LanmanServer >nul
call :OpenFirewall
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:Fix7e
echo --- Fix 0x0000007e ---
call :CopyMSCMS
net stop spooler >nul 2>&1
del /q /f "%SystemRoot%\System32\spool\PRINTERS\*.*" >nul 2>&1
call :RestartSpooler
echo [ACTION] Them lai may in de Windows tu dong tai Driver.
pause & goto MainMenu

:Fix12
echo --- Fix 0x00000012 ---
net stop spooler >nul 2>&1
del /q /f "%SystemRoot%\System32\spool\PRINTERS\*.*" >nul 2>&1
sc config spooler start= auto >nul
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:Fix3eb
echo --- Fix 0x000003eb ---
dism /Online /Enable-Feature /FeatureName:Printing-Foundation-InternetPrinting-Client /NoRestart
dism /Online /Enable-Feature /FeatureName:Printing-LPRPortMonitor /NoRestart
dism /Online /Enable-Feature /FeatureName:Printing-Foundation-LPDPrintService /NoRestart
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:Fix771
echo --- Fix 0x00000771 (Printer Offline) ---
echo Dang chuyen may in sang trang thai Online...
powershell -Command "$p = Get-WmiObject -Class Win32_Printer | Where-Object { $_.WorkOffline -eq$true }; foreach ($i in$p) { $i.WorkOffline =$false; $i.Put() \vert{} Out-Null; Write-Host 'Da Online:'$i.Name }"
net stop spooler >nul 2>&1
del /q /f "%SystemRoot%\System32\spool\PRINTERS\*.*" >nul 2>&1
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:AddCred
echo --- Them Credential ---

:InputSrv
set "srv="
set /p srv="Nhap IP hoac Ten may chu (VD: 192.168.1.10): "
set "srv=%srv: =%"
if "%srv%"=="" goto SrvEmpty
if "%srv:~1,1%"=="" goto SrvShort
> "%temp%\srv_check.txt" echo "%srv%"
findstr /R /I /V "^.[a-z0-9._-]*.$" "%temp%\srv_check.txt" >nul
if %errorlevel% equ 0 goto SrvInvalid
del /q "%temp%\srv_check.txt" >nul 2>&1
goto InputUsr

:SrvEmpty
echo [!] Khong duoc de trong! Vui long nhap lai.
goto InputSrv

:SrvShort
echo [!] LOI: Ten may hoac IP phai dai tu 2 ky tu tro len!
goto InputSrv

:SrvInvalid
echo [!] LOI: Phat hien ky tu dac biet hoac dau tieng Viet!
echo [!] Chi cho phep: Chu cai, So, Dau cham, Gach ngang, Gach duoi.
del /q "%temp%\srv_check.txt" >nul 2>&1
goto InputSrv

:InputUsr
set "usr="
set /p usr="Nhap Username: "
if "%usr%"=="" (
    echo [!] Username khong duoc de trong! Vui long nhap lai.
    goto InputUsr
)

:InputPwd
set "pwd="
set /p pwd="Nhap Mat khau: "
if "%pwd%"=="" (
    echo [!] Mat khau khong duoc de trong! Vui long nhap lai.
    goto InputPwd
)

cmdkey /add:%srv% /user:%usr% /pass:%pwd%
echo [OK] Da them credential thanh cong cho: %srv%
pause & goto MainMenu

:ViewCred
echo --- Xem Credential ---
cmdkey /list
pause & goto MainMenu

:DelCred
echo --- Xoa Credential ---
cmdkey /list
echo.
set /p srv="Nhap Ten/IP may chu can xoa (ghi dung phan Target): "
cmdkey /delete:%srv%
echo [OK] Hoan tat!
pause & goto MainMenu

:RestartSpoolerOnly
echo --- Restart Print Spooler ---
call :RestartSpooler
pause & goto MainMenu

:ClearSpool
echo --- Xoa Hang Doi In ---
net stop spooler >nul 2>&1
del /q /f "%SystemRoot%\System32\spool\PRINTERS\*.*" >nul 2>&1
call :RestartSpooler
echo [OK] Da xoa sach file tam spooler.
pause & goto MainMenu

:ResetUSBMon
echo --- Reset USB Monitor ---
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Print\Monitors\USB Monitor" /f >nul 2>&1
echo [OK] Da xoa USB Monitor.
echo [ACTION] RUT CAP USB VA CAM LAI.
pause & goto MainMenu

:FixSpoolerSvc
echo --- Fix Print Spooler Service ---
net stop spooler >nul 2>&1
taskkill /f /im spoolsv.exe >nul 2>&1
del /q /f "%SystemRoot%\System32\spool\PRINTERS\*.*" >nul 2>&1
sc config spooler start= auto >nul
sc sdset spooler D:(A;;CCLCSWRPWPDTLOCRRC;;;SY)(A;;CCDCLCSWRPWPDTLOCRSDRCWDWO;;;BA)(A;;CCLCSWLOCRRC;;;IU)(A;;CCLCSWLOCRRC;;;SU) >nul
call :RestartSpooler
echo [OK] Da khoi phuc quyen va khoi dong lai Spooler.
pause & goto MainMenu

:ResetPorts
echo --- Reset PrinterPorts (Chi Xoa IP Loi) ---
powershell -Command "Get-Item 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Ports' | Select-Object -ExpandProperty Property | Where-Object { $_ -notmatch '(?i)^(usb|lpt|com|file:|nul|portprompt|hklm|xps)' -and ($_ -match '\.' -or$_ -match '\\\\') } | ForEach-Object { Remove-ItemProperty -Path 'HKLM:\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Ports' -Name $_ -Force; Write-Host 'Da xoa port loi:'$_ }"
call :RestartSpooler
echo [OK] Hoan tat. (Giu nguyen cong USB/LPT/COM).
pause & goto MainMenu

:SetLocalConn
echo --- Set LocalConnection ---
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v forceguest /t REG_DWORD /d 0 /f >nul
reg add "HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" /v AllowInsecureGuestAuth /t REG_DWORD /d 1 /f >nul
call :SetRPC
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:FixCanon2900
echo --- Fix Canon LBP 2900/3300 (Full Reset) ---
net stop spooler >nul 2>&1
taskkill /f /im spoolsv.exe >nul 2>&1
del /q /f "%SystemRoot%\System32\spool\PRINTERS\*.*" >nul 2>&1
reg delete "HKLM\SYSTEM\CurrentControlSet\Control\Print\Monitors\USB Monitor" /f >nul 2>&1
call :DeleteCNBJNP
call :CopyMSCMS
call :RestartSpooler
echo [ACTION] RUT CAP USB, CHO 10 GIAY, CAM LAI DE MAY IN NHAN LAI DRIVER!
pause & goto MainMenu

:UnlockShare
echo --- Unlock Share Printer ---
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers" /v DisableHTTPPrinting /t REG_DWORD /d 0 /f >nul
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers" /v DisablePrinterAdmin /t REG_DWORD /d 0 /f >nul
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v RestrictDriverInstallationToAdministrators /t REG_DWORD /d 0 /f >nul
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v NoWarningNoElevationOnInstall /t REG_DWORD /d 1 /f >nul
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v UpdatePromptSettings /t REG_DWORD /d 2 /f >nul
call :RestartSpooler
echo [OK] Hoan tat!
pause & goto MainMenu

:DelCanonDriver
echo --- Xoa Driver Canon LBP Cu ---
echo Vui long cho trong giay lat...
pnputil /enum-drivers > "%temp%\drv_list.txt"
findstr /i "canon lbp" "%temp%\drv_list.txt" >nul
if %errorlevel% == 0 (
    echo [INFO] Tim thay driver Canon. Ban hay chay lenh sau de xoa:
    echo        pnputil /delete-driver oemXX.inf /uninstall /force
    echo        ^(Thay XX bang so tuong ung hoac tim trong Device Manager^).
) else (
    echo [INFO] Khong tim thay Driver Canon LBP.
)
pause & goto MainMenu

:AutoFix15
echo --- AUTO FIX 15 BUOC - Toan Dien ---
echo 1. Bat Windows Firewall...
sc config MpsSvc start= auto >nul & sc start MpsSvc >nul
echo 2. Mo FW File and Printer...
call :OpenFirewall
echo 3-6. Cau hinh RPC...
call :SetRPC
echo 7-8. Go chan PointAndPrint...
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v RestrictDriverInstallationToAdministrators /t REG_DWORD /d 0 /f >nul
reg add "HKLM\SOFTWARE\Policies\Microsoft\Windows NT\Printers\PointAndPrint" /v NoWarningNoElevationOnInstall /t REG_DWORD /d 1 /f >nul
echo 9-10. Cau hinh Guest Auth...
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Lsa" /v forceguest /t REG_DWORD /d 0 /f >nul
reg add "HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters" /v AllowInsecureGuestAuth /t REG_DWORD /d 1 /f >nul
echo 11. Copy mscms.dll...
call :CopyMSCMS
echo 12. Bat dich vu mang chinh...
sc config LanmanWorkstation start= auto >nul & sc start LanmanWorkstation >nul
sc config LanmanServer start= auto >nul & sc start LanmanServer >nul
echo 13. Xoa hang doi in...
net stop spooler >nul 2>&1
del /q /f "%SystemRoot%\System32\spool\PRINTERS\*.*" >nul 2>&1
echo 14. Bat dich vu mang phu tro...
for %%A in (fdPHost FDResPub SSDPSRV upnphost) do (sc config %%A start= auto >nul & sc start %%A >nul)
echo 15. Restart Print Spooler...
call :RestartSpooler
echo [OK] AUTO FIX HOAN TAT. Thu ket noi lai may in!
pause & goto MainMenu

:OpenMgmt
echo --- Mo Cong Cu Quan Ly ---
echo 1. Device Manager
echo 2. Print Management
echo 3. Print Server Properties
set /p mgmt="Chon (1-3): "
if "%mgmt%"=="1" start devmgmt.msc
if "%mgmt%"=="2" start printmanagement.msc
if "%mgmt%"=="3" start rundll32 printui.dll,PrintUIEntry /s /t2
goto MainMenu
```




**Bước 3:** Trên Notepad, chọn File > Save As...
<img width="1176" height="602" alt="image" src="https://github.com/user-attachments/assets/1ec79ccf-2775-44c8-8ff5-e0d0c9f8d989" />


*   **Tại mục File name:** Đặt tên là ToolFixMayIn.bat (Lưu ý phải có đuôi .bat).


*   **Tại mục Save as type:** Chọn All files (.).


*   **Tại mục Encoding (bên cạnh nút Save):** Chọn UTF-8 (Để hiển thị tiếng Việt không bị lỗi font).


*   **Bấm Save:** để lưu ra màn hình Desktop.

<img width="944" height="590" alt="image" src="https://github.com/user-attachments/assets/122a3781-998a-4e67-9912-72d7297cc378" />


## 3. Hướng dẫn sử dụng

Tại màn hình Desktop, click đúp chuột vào file ToolFixMayIn.bat vừa tạo. (Công cụ đã được lập trình để tự động yêu cầu cấp quyền Administrator nếu bạn mở theo cách thông thường).

<img width="448" height="374" alt="image" src="https://github.com/user-attachments/assets/98dc77bd-448b-4d9b-a954-770cadf3fe2a" />

<img width="1474" height="764" alt="image" src="https://github.com/user-attachments/assets/d54fc44e-368a-4e93-9bee-5222e0f5facf" />

*   **Mẹo**: Bạn cũng có thể click chuột phải vào file > Chọn Run as administrator. Sau khi bảng điều khiển CMD hiện lên, bạn sẽ thấy giao diện Menu gồm 34 chức năng chia theo từng nhóm:


*   **Nhóm [SERVER]**: Bấm các số từ 1 đến 5 nếu bạn đang ngồi ở máy chủ (máy tính cắm dây cáp USB trực tiếp vào máy in và đang muốn chia sẻ cho máy khác).


*   **Nhóm [CLIENT]**: Bấm các số từ 10 đến 20 nếu bạn đang ngồi ở máy khách (máy tính muốn kết nối qua mạng WiFi/LAN tới máy chủ nhưng báo lỗi).


*   **Nhóm [CÔNG CỤ BỔ SUNG]**: Các phím tắt tiện ích như xóa lệnh in bị kẹt (số 25), khởi động lại tiến trình in (số 24).


*   **Cách chạy:** Nhập số tương ứng với tình trạng lỗi của bạn tại dòng Chon chuc nang (0-34): và nhấn Enter.


*   **LỜI KHUYÊN:** Nếu bạn không rành về mã lỗi, hãy gõ số 33 và nhấn Enter. Chức năng [★ AUTO FIX 15 BUOC] sẽ tự động làm mới tường lửa, thiết lập lại registry bảo mật, khởi động các dịch vụ mạng và dọn dẹp hàng đợi in giúp bạn chỉ trong vài giây.


Làm theo thông báo trên màn hình (Ví dụ: rút cáp USB rồi cắm lại nếu sửa lỗi máy in Canon). Bấm phím bất kỳ để quay lại Menu chính khi hoàn thành. Nhập số 0 để thoát chương trình.</script>
