# The Inaccuracies of Antivirus Software

This repository demonstrates how antivirus software can incorrectly flag completely harmless programs as malicious.

### The Experiment

1. A simple "Hello, World!" program is created in C++. See [/test/test.cpp](./test/test.cpp) for the source code.
2. The program is compiled into an executable file in Release mode, which compiles it for optimization and performance.
3. The compiled executable is then scanned using various antivirus software to see if it gets flagged as malicious.

Using [VirusTotal](https://www.virustotal.com/), the compiled executable was scanned against several different antivirus
engines. 

### Results

[VirusTotal Link](https://www.virustotal.com/gui/file/73aa68c38a8f79391d1fab7784125c0adfbdab51eca68fe365611c451d006f0c/detection)
[Binary File](https://github.com/reversed-coffee/false-positive-lab/releases/download/1.0.0/test.exe)

Out of the 72 antivirus programs tested, **four** of them flagged the harmless "Hello, World!" executable as malicious:

| Provider              | Classification               |
|-----------------------|------------------------------|
| DeepInstinct          | MALICIOUS                    |
| MaxSecure             | Trojan.Malware.300983.susgen |
| SecureAge             | Malicious                    |
| Symantec              | ML.Attribute.HighConfidence  |

### Analysis

These detections are false positives. Many antivirus engines rely on generic behavioral patterns or maching learning
heuristics. Terms like `susgen` (suspicious generic) and `ML.Attribute` (machine learning attribute) indicate that the
"detection" is likely a result of heuristic analysis rather than a specific signature match, **not a confirmation of
malicious intent**.

The program scanned exhibits **no malicious behavior**; it simply prints "Hello, World!" to the console, yet it was
still flagged by antivirus software. What causes these false positives?

1. **Unsigned Executables**: Many antivirus programs are more likely to flag unsigned executables, as they cannot verify
   the publisher's identity. Released builds without code signing are often treated with suspicion.
2. **Heuristic Analysis**: Antivirus software often uses heuristic analysis to identify potential threats based on
   behavior patterns, which can lead to false positives. Heuristics are often flawed and misclassify benign programs.

Executables that are unsigned are not necessarily malicious. Many developers do not want to pay a significant amount of
money for code signing certificates, especially for small or open-source projects. Keep in mind that code signing
is only putting trust in the publisher, not in the actual software itself. True threat actors know this and often sign
their malware with valid certificates to bypass security checks.

Dynamic analysis and behavior monitoring tools like [Tria.ge](https://tria.ge/) provide a more accurate assessment of
a program's behavior in a controlled environment, reducing the likelihood of false positives. However, people may take
its "detections" out of context. For example, many bootstrapper programs (like installers) may be flagged for being
"droppers" because they write executable files to the disk, but this is a normal behavior for installers. **Please do
your own research on what specifically is being detected before jumping to conclusions.**

### Key Takeways

- Antivirus software can be inaccurate and can produce false positives, flagging harmless programs as malicious.
- Relying solely on antivirus software for security can lead to unnecessary alarm and wasted resources.
- Do not assume that an unsigned executable is inherently malicious; many legitimate programs are unsigned.
- Rely more on dynamic analysis and behavior monitoring like [Tria.ge](https://tria.ge/) for better threat detection.

### Disclaimer

This repository is intended for educational purposes only. The author is not responsible for any misuse of the
information provided herein.
