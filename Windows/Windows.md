# Windows

## 秘钥

[参考](https://learn.microsoft.com/zh-cn/windows-server/get-started/kms-client-activation-keys)

### Windows 11

#### 专业工作站版

```text
8D2T7-WN7YB-8M4X7-8WVY7-94VBX
```

## 脚本 

```text
@echo off
mode con cols=75 lines=25
title 沧水的KMS脚本
setlocal EnableDelayedExpansion&color 70 & cd /d "%~dp0"
%1 %2
ver|find "5."> NUL&&goto :start
mshta vbscript:createobject("shell.application").shellexecute("%~s0","goto :start","","runas",1)(window.close)&goto :eof
:start
echo 本脚本需要右键管理员运行
echo 提问建议请留言http://kms.cangshui.net
echo 捐赠赞助请访问http://shop.cangshui.net
set /p xuanze=【A】KMS激活Windows   【B】KMS激活Office 【C】清除Windows KMS 【D】清除Office KMS  请输入你的选择:
if /i "%xuanze%"=="a" cls&goto start1
if /i "%xuanze%"=="b" cls&goto start2
if /i "%xuanze%"=="c" cls&goto start3
if /i "%xuanze%"=="d" cls&goto start4

:start2
set KMS_Sev=kms.cangshui.net
cls
echo 正在检查本机网络状态...
echo.
ping www.microsoft.com | find "超时"  > NUL &&  goto fail
ping www.microsoft.com | find "目标主机"  > NUL &&  goto fail
echo 本机网络良好……
goto office

:office
echo 检查安装的office……
call :GetOfficePath 14 Office2010
call :ActOffice 14 Office2010
call :GetOfficePath 15 Office2013
call :ActOffice 15 Office2013
if exist "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" set _Office16Path=%ProgramFiles%\Microsoft Office\Office16
if exist "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" set _Office16Path=%ProgramFiles(x86)%\Microsoft Office\Office16
if DEFINED _Office16Path (echo.&echo 已发现 Office2016系列软件[包括2016/2019/365]
    call :ActOffice 16 Office2016
  ) else (echo.&echo 未发现 Office2016系列软件[包括2016/2019/365])


echo.&pause
exit

:ActOffice
if DEFINED _Office%1Path (
    cd /d "!_Office%1Path!"
    if %1 EQU 16 call :Licens16
    echo.&echo 尝试激活您的Office ...&echo.
cscript //nologo ospp.vbs /sethst:kms.cangshui.net > NUL
cscript //nologo ospp.vbs /act | find /i "successful" && (
        echo.&echo ***** 激活成功 *****   & echo.) || (echo.&echo ***** 激活失败 ***** & echo.)
)    
cd /d "%~dp0"
goto :EOF

:GetOfficePath
echo.&echo 正在检测 %2 系列产品的安装路径...
set _Office%1Path=
set _Reg32=HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Office\%1.0\Common\InstallRoot
set _Reg64=HKEY_LOCAL_MACHINE\SOFTWARE\Wow6432Node\Microsoft\Office\%1.0\Common\InstallRoot
reg query "%_Reg32%" /v "Path" > nul 2>&1 && FOR /F "tokens=2*" %%a IN ('reg query "%_Reg32%" /v "Path"') do SET "_OfficePath1=%%b"
reg query "%_Reg64%" /v "Path" > nul 2>&1 && FOR /F "tokens=2*" %%a IN ('reg query "%_Reg64%" /v "Path"') do SET "_OfficePath2=%%b"
if DEFINED _OfficePath1 (if exist "%_OfficePath1%ospp.vbs" set _Office%1Path=!_OfficePath1!)
if DEFINED _OfficePath2 (if exist "%_OfficePath2%ospp.vbs" set _Office%1Path=!_OfficePath2!)
set _OfficePath1=
set _OfficePath2=
if DEFINED _Office%1Path (echo.&echo 已发现 %2) else (echo.&echo 未发现 %2)
goto :EOF

:Licens16

set /p xuanze=【A】激活为Office2019版本(仅2019、2021与365版本可选)   【B】激活为Office2016版本（通用）
if /i "%xuanze%"=="a" cls&goto installOffice19
if /i "%xuanze%"=="b" cls&goto installOffice16


:installOffice19
echo 安装2019证书
for /f %%x in ('dir /b ..\root\Licenses16\ProPlus2019VL_KMS_Client_AE-ppd.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\ProPlus2019VL_KMS_Client_AE-ul.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\ProPlus2019VL_KMS_Client_AE-ul-oob.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\client-issuance-bridge-office.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\client-issuance-root.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\client-issuance-root-bridge-test.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\client-issuance-stil.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\client-issuance-ul.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\client-issuance-ul-oob.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\pkeyconfig-office.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
cscript "%_Office16Path%\ospp.vbs" /inpkey:NMMKJ-6RK4F-KMJVX-8D9MJ-6MWKP > NUL

goto :EOF
exit
:fail
cls
echo 无法连接到服务器，可尝试重新运行脚本......
pause

:installOffice16
echo 安装2016证书
for /f %%x in ('dir /b ..\root\Licenses16\project???vl_kms*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\proplusvl_kms*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\standardvl_kms*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\visio???vl_kms*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\project???vl_mak*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\proplusvl_mak*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\standardvl_mak*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
for /f %%x in ('dir /b ..\root\Licenses16\visio???vl_mak*.xrm-ms') do cscript ospp.vbs /inslic:"..\root\Licenses16\%%x" > NUL
cscript "%_Office16Path%\ospp.vbs" /inpkey:NMMKJ-6RK4F-KMJVX-8D9MJ-6MWKP > NUL
cscript "%_Office16Path%\ospp.vbs" /inpkey:NYH39-6GMXT-T39D4-WVXY2-D69YY > NUL
cscript "%_Office16Path%\ospp.vbs" /inpkey:7WHWN-4T7MP-G96JF-G33KR-W8GF4 > NUL
cscript "%_Office16Path%\ospp.vbs" /inpkey:RBWW7-NTJD4-FFK2C-TDJ7V-4C2QP > NUL
cscript "%_Office16Path%\ospp.vbs" /inpkey:XQNVK-8JYDB-WJ9W3-YJ8YR-WFG99 > NUL
cscript "%_Office16Path%\ospp.vbs" /inpkey:YG9NW-3K39V-2T3HJ-93F3Q-G83KT > NUL
cscript "%_Office16Path%\ospp.vbs" /inpkey:PD3PC-RHNGV-FXJ29-8JK7D-RJRJK > NUL
goto :EOF

exit
:fail
cls
echo 无法连接到服务器，可尝试重新运行脚本......
pause




:start1
set KMS_Sev=kms.cangshui.net
cls
echo 提问建议请留言http://kms.cangshui.net
echo 捐赠赞助请访问http://shop.cangshui.net
echo 正在检查本机网络状态...
echo.
ping www.microsoft.com | find "超时"  > NUL &&  goto fail
ping www.microsoft.com | find "目标主机"  > NUL &&  goto fail
echo 本机网络良好……

echo =================================激活信息==================================

ver | find "6.0." > NUL &&  goto winvista
ver | find "6.1." > NUL &&  goto win7
ver | find "6.2." > NUL &&  goto win8
ver | find "6.3." > NUL &&  goto win81
ver | find "10.0." > NUL &&  goto win10
echo 未找到合适的NT6系统，可能是WinXP或Win2003。
goto office

:winvista
echo 当前为Windows Vista/2008。
set Business=YFKBB-PQJJV-G996G-VWGXY-2V3X8
set BusinessN=HMBQG-8H2RH-C77VX-27R82-VMQBT
set Enterprise=VKK3X-68KWM-X2YGT-QR4M6-4BWMV
set EnterpriseN=VTC42-BM838-43QHV-84HX6-XJXKV
set ServerWeb=WYR28-R7TFJ-3X2YQ-YCY4H-M249D
set ServerStandard=TM24T-X9RMF-VWXK6-X8JC9-BFGM2
set ServerStandardV=W7VD6-7JFBR-RX26B-YKQ3Y-6FFFJ
set ServerEnterprise=YQGMW-MPWTJ-34KDK-48M3W-X4Q6V
set ServerEnterpriseV=39BXF-X8Q23-P2WWT-38T2F-G3FPG
set ServerWeb=RCTX3-KWVHP-BR6TB-RB6DM-6X7HP
set ServerDatacenter=7M67G-PC374-GR742-YH8V4-TCBY3
set ServerDatacenterV=22XQ2-VRXRG-P8D42-K34TD-G3QQC
set ServerEnterpriseIA64=4DWFP-JF3DJ-B7DTH-78FJB-PDRHK
goto windowsstart

:win7
echo 当前为Windows 7/2008 R2。
set Professional=FJ82H-XT6CR-J8D7P-XQJJ2-GPDD4
set ProfessionalN=MRPKT-YTG23-K7D7T-X2JMM-QY7MG
set ProfessionalE=W82YF-2Q76Y-63HXB-FGJG9-GF7QX
set Enterprise=33PXH-7Y6KF-2VJC9-XBBR8-HVTHH
set EnterpriseN=YDRBP-3D83W-TY26F-D46B2-XCKRJ
set EnterpriseE=C29WB-22CC8-VJ326-GHFJW-H9DH4
set ServerWeb=6TPJF-RBVHG-WBW2R-86QPH-6RTM4
set ServerHPC=TT8MH-CG224-D3D7Q-498W2-9QCTX
set ServerStandard=YC6KT-GKW9T-YTKYR-T4X34-R7VHC
set ServerEnterprise=489J6-VHDMP-X63PK-3K798-CPX3Y
set ServerDatacenter=74YFP-3QFB3-KQT8W-PMXWJ-7M648
set ServerEnterpriseIA64=GT63C-RJFQ3-4GMB6-BRFB9-CB83V
goto windowsstart
:win8
echo 当前为Windows 8/2012。
set Professional=NG4HW-VH26C-733KW-K6F98-J8CK4
set ProfessionalN=XCVCF-2NXM9-723PB-MHCB7-2RYQQ
set Core=BN3D2-R7TKB-3YPBD-8DRP2-27GG4
set Enterprise=32JNW-9KQ84-P47T8-D8GGY-CWCK7
set EnterpriseN=JMNMF-RHW7P-DMY6X-RF3DR-X2BQT
set CoreN=8N2M2-HWPGY-7PGT9-HGDD8-GVGGY
set CoreSingleLanguage=2WN2H-YGCQR-KFX6K-CD6TF-84YXQ
set CoreCountrySpecific=4K36P-JN4VD-GDC6V-KDT89-DYFKP
set ServerMultiPointPremium=XNH6W-2V9GX-RGJ4K-Y8X6F-QGJ2G
set ServerMultiPointStandard=HM7DN-YVMH3-46JC3-XYTG7-CYQJJ
set ServerStandard=XC9B7-NBPP2-83J2H-RHMBY-92BT4
set ServerDatacenter=48HP8-DN98B-MYWDG-T2DCC-8W83P
goto windowsstart
:win81
echo 当前为Windows 8.1。
set Professional=GCRJD-8NW9H-F2CDX-CCM8D-9D6T9
set ProfessionalN=HMCNV-VVBFX-7HMBH-CTY9B-B4FXY
set Enterprise=MHF9N-XY6XB-WVXMC-BTDCT-MKKG7
set EnterpriseN=TT4HM-HN7YT-62K67-RGRQJ-JFFXW
set ServerSolution=KNC87-3J2TX-XB4WP-VCPJV-M4FWM
set ServerStandard=D2N9P-3P6X9-2R39C-7RTCD-MDVJX
set ServerDatacenter=W3GGN-FT8W3-Y4M27-J84CP-Q3VJ9
set EmbeddedIndustry=32JNW-9KQ84-P47T8-D8GGY-CWCK7
goto windowsstart
:win10
echo 当前为Windows 10/Server 2016-2019。
set Core=TX9XD-98N7V-6WMQ6-BX7FG-H8Q99
set CoreCountrySpecific=PVMJN-6DFY6-9CCP6-7BKTT-D3WVR
set CoreN=3KHY7-WNT83-DGQKR-F7HPR-844BM
set CoreSingleLanguage=7HNRX-D7KGG-3K4RQ-4WPJ4-YTDFH
set Professional=W269N-WFGWX-YVC9B-4J6C9-T83GX
set ProfessionalN=MH37W-N47XK-V7XM9-C7227-GCQG9
set Enterprise=NPPR9-FWDCX-D2C8J-H872K-2YT43
set EnterpriseN=DPH2V-TTNVB-4X9Q3-TJR4H-KHJW4
set Education=NW6C2-QMPVW-D7KKK-3GKT6-VCFB2
set EducationN=2WH4N-8QGBV-H22JP-CT43Q-MDWWJ
set EnterpriseS=WNMTR-4C88C-JK8YV-HQ7T2-76DF9
set EnterpriseSN=2F77B-TNFGY-69QQF-B8YKP-D69TJ
set ServerDatacenter=CB7KF-BWN84-R7R2Y-793K2-8XDDG
set ServerStandard=WC2BQ-8NRM3-FDDYY-2BFGV-KHKQY
set ServerEssentials=JCKRF-N37P4-C2D82-9YXRT-4M63B
set EnterpriseG=YYVX9-NTFWV-6MDM3-9PT4T-4M68B
set EnterpriseGN=44RPN-FTY23-9VTTB-MP9BX-T84FV
set ProfessionalEducation=6TP4R-GNPTD-KYYHQ-7B7DP-J447Y
set ProfessionalEducationN=YVWGF-BXNMC-HTQYQ-CPQ99-66QFC
set ProfessionalWorkstation=NRG8B-VKK3Q-CXVCJ-9G2XF-6Q84J
set ProfessionalWorkstations=NRG8B-VKK3Q-CXVCJ-9G2XF-6Q84J
set ProfessionalWorkstationsN=9FNHH-K3HBT-3W4TD-6383H-6XYWF
set ServerDatacenter=WMDGN-G9PQG-XVVXX-R3X43-63DFG
set ServerStandard=N69G4-B89J2-4G8F4-WWYCC-J464C
set ServerEssentials=WVDHN-86M7X-466P6-VHXV7-YY726
set ServerRdsh=CPWHC-NT2C7-VYW78-DHDB2-PG3GK

goto windowsstart
:windowsstart
for /f "tokens=3 delims= " %%i in ('reg QUERY "HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion" /v "EditionID"') do set EditionID=%%i
if defined %EditionID% (
    echo 版本ID为%EditionID%
	cscript //Nologo %windir%\system32\slmgr.vbs /ipk !%EditionID%!
	cscript //Nologo %windir%\system32\slmgr.vbs /skms kms.cangshui.net
	cscript //Nologo %windir%\system32\slmgr.vbs /ato
) else (
	echo 找不到序列号，可能是旗舰版之类的系统……
)

echo =================================激活信息==================================

echo.&pause
exit

:start4
set /p xuanze=是否真的要清除Office的KMS激活？【Y】继续   【N】关闭

if /i "%xuanze%"=="y" goto nextun
if /i "%xuanze%"=="n" exit
:nextun

if exist "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs"  (
cscript "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" /unpkey:6MWKP
cscript "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" /unpkey:D69YY
cscript "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" /unpkey:W8GF4
cscript "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" /unpkey:4C2QP
cscript "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" /unpkey:WFG99
cscript "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" /unpkey:G83KT
cscript "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" /unpkey:RJRJK
cscript "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" /unpkey:DDBV6
cscript "%ProgramFiles%\Microsoft Office\Office16\ospp.vbs" /unpkey:VMFTK
)

if exist "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs"  (
cscript "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" /unpkey:6MWKP
cscript "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" /unpkey:D69YY
cscript "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" /unpkey:W8GF4
cscript "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" /unpkey:4C2QP
cscript "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" /unpkey:WFG99
cscript "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" /unpkey:G83KT
cscript "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" /unpkey:RJRJK
cscript "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" /unpkey:DDBV6
cscript "%ProgramFiles(x86)%\Microsoft Office\Office16\ospp.vbs" /unpkey:VMFTK
)
if exist "%ProgramFiles%\Microsoft Office\Office15\ospp.vbs"   ( 
cscript "%ProgramFiles%\Microsoft Office\Office15\ospp.vbs" /unpkey:6MWKP
cscript "%ProgramFiles%\Microsoft Office\Office15\ospp.vbs" /unpkey:D69YY
cscript "%ProgramFiles%\Microsoft Office\Office15\ospp.vbs" /unpkey:W8GF4
cscript "%ProgramFiles%\Microsoft Office\Office15\ospp.vbs" /unpkey:4C2QP
cscript "%ProgramFiles%\Microsoft Office\Office15\ospp.vbs" /unpkey:WFG99
cscript "%ProgramFiles%\Microsoft Office\Office15\ospp.vbs" /unpkey:G83KT
cscript "%ProgramFiles%\Microsoft Office\Office15\ospp.vbs" /unpkey:RJRJK
cscript "%ProgramFiles%\Microsoft Office\Office15\ospp.vbs" /unpkey:DDBV6
cscript "%ProgramFiles%\Microsoft Office\Office15\ospp.vbs" /unpkey:VMFTK
)
if exist "%ProgramFiles(x86)%\Microsoft Office\Office15\ospp.vbs"   ( 
cscript "%ProgramFiles(x86)%\Microsoft Office\Office15\OSPP.VBS" /unpkey:6MWKP
cscript "%ProgramFiles(x86)%\Microsoft Office\Office15\OSPP.VBS" /unpkey:D69YY
cscript "%ProgramFiles(x86)%\Microsoft Office\Office15\OSPP.VBS" /unpkey:W8GF4
cscript "%ProgramFiles(x86)%\Microsoft Office\Office15\OSPP.VBS" /unpkey:4C2QP
cscript "%ProgramFiles(x86)%\Microsoft Office\Office15\OSPP.VBS" /unpkey:WFG99
cscript "%ProgramFiles(x86)%\Microsoft Office\Office15\OSPP.VBS" /unpkey:G83KT
cscript "%ProgramFiles(x86)%\Microsoft Office\Office15\OSPP.VBS" /unpkey:RJRJK
cscript "%ProgramFiles(x86)%\Microsoft Office\Office15\OSPP.VBS" /unpkey:DDBV6
cscript "%ProgramFiles(x86)%\Microsoft Office\Office15\OSPP.VBS" /unpkey:VMFTK
)
if exist "%ProgramFiles%\Microsoft Office\Office14\ospp.vbs"  (
cscript "%ProgramFiles%\Microsoft Office\Office14\ospp.vbs" /unpkey:6MWKP
cscript "%ProgramFiles%\Microsoft Office\Office14\ospp.vbs" /unpkey:D69YY
cscript "%ProgramFiles%\Microsoft Office\Office14\ospp.vbs" /unpkey:W8GF4
cscript "%ProgramFiles%\Microsoft Office\Office14\ospp.vbs" /unpkey:4C2QP
cscript "%ProgramFiles%\Microsoft Office\Office14\ospp.vbs" /unpkey:WFG99
cscript "%ProgramFiles%\Microsoft Office\Office14\ospp.vbs" /unpkey:G83KT
cscript "%ProgramFiles%\Microsoft Office\Office14\ospp.vbs" /unpkey:RJRJK
cscript "%ProgramFiles%\Microsoft Office\Office14\ospp.vbs" /unpkey:DDBV6
cscript "%ProgramFiles%\Microsoft Office\Office14\ospp.vbs" /unpkey:VMFTK
)
if exist "%ProgramFiles(x86)%\Microsoft Office\Office14\ospp.vbs"  (
cscript "%ProgramFiles(x86)%\Microsoft Office\Office14\ospp.vbs" /unpkey:6MWKP
cscript "%ProgramFiles(x86)%\Microsoft Office\Office14\ospp.vbs" /unpkey:D69YY
cscript "%ProgramFiles(x86)%\Microsoft Office\Office14\ospp.vbs" /unpkey:W8GF4
cscript "%ProgramFiles(x86)%\Microsoft Office\Office14\ospp.vbs" /unpkey:4C2QP
cscript "%ProgramFiles(x86)%\Microsoft Office\Office14\ospp.vbs" /unpkey:WFG99
cscript "%ProgramFiles(x86)%\Microsoft Office\Office14\ospp.vbs" /unpkey:G83KT
cscript "%ProgramFiles(x86)%\Microsoft Office\Office14\ospp.vbs" /unpkey:RJRJK
cscript "%ProgramFiles(x86)%\Microsoft Office\Office14\ospp.vbs" /unpkey:DDBV6
cscript "%ProgramFiles(x86)%\Microsoft Office\Office14\ospp.vbs" /unpkey:VMFTK
)
ping 127.0.0.1 -n 1 > nul
cscript "C:\Program Files\Microsoft Office\Office16\OSPP.VBS" /remhst
cls
echo 清除完成
ping 127.0.0.1 -n 10 > nul
exit


:start3
set /p xuanze=是否真的要清除Windows的KMS？【Y】继续   【N】关闭
if /i "%xuanze%"=="y" goto nextunw
if /i "%xuanze%"=="n" exit
:nextunw
slmgr /upk
slmgr /ckms
slmgr /rearm
cls
echo 清除完成，请重启电脑
ping 127.0.0.1 -n 10 > nul




```