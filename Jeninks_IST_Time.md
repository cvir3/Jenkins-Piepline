## Jenkins Time Zone Configuration (IST Fix)
- Even after setting the time zone in the Jenkins UI, Jenkins may still run using UTC.
```bash
Profile → Preferences → Time zone → Asia/Kolkata
```
- This setting only affects the UI display and does not change the system time zone used by Jenkins internally.


## Solution

- To force Jenkins to use IST (Asia/Kolkata), you need to add a JVM argument in the Jenkins configuration.

## Steps
1. Navigate to the Jenkins installation directory:
```bash
C:\Program Files\Jenkins
```
2. Locate the file:
```bahs
jenkins.xml
```
3. Open the file in any code editor (e.g., Notepad++, VS Code).
4. Find the 'arguments' section.
5. Add the following JVM argument inside it.
```bash
-Duser.timezone=Asia/Kolkata
```
- It is displayed like this.
```bash
<arguments>-Xrs -Xmx256m ... -Duser.timezone=Asia/Kolkata</arguments>
```
6. Save the file
7. Restart the Jenkins service

> [!NOTE] 
> - Jenkins will now run using the IST (Asia/Kolkata) time zone instead of UTC.