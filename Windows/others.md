## Windows Defenderの停止
```powershell
Set-MpPreference -DisableBehaviorMonitoring $true
Set-MpPreference -DisableIntrusionPreventionSystem $true
Set-MpPreference -DisableIOAVProtection $true
Set-MpPreference -DisableBlockAtFirstSeen $true
```
