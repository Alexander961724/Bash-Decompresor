# Bash-Decompresor
This Bash script recursively extracts nested compressed files using **7z** until no more archives remain.
🗂️ Bash-Decompresor
🔹 Description
A simple Bash script that recursively extracts nested compressed files using 7z until no more archives remain.

🛠️ How It Works
1️⃣ Extracts first_file_name using 7z.
2️⃣ Checks if the extracted file is another compressed archive.
3️⃣ If yes, extracts it too.
4️⃣ Repeats until no more compressed files remain.

📜 Script Code
bash
Copiar
Editar
#!/bin/bash
endColour="\033[0m\e[0m"
redColour="\e[0;31m\033[1m"
yellowColour="\e[0;33m\033[1m"

function ctrl_c() {
    echo -e "\n\n${redColour}[!]${endColour} Exiting..\n"
}

first_file_name="data.gz"
decompresed_file_name="$(7z l $first_file_name | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"
7z x $first_file_name &>/dev/null

while [ "$decompresed_file_name" ]; do
    echo -e "\n${yellowColour}[+]${endColour} New extracted file: $decompresed_file_name"
    7z x $decompresed_file_name &>/dev/null
    decompresed_file_name="$(7z l $decompresed_file_name 2>/dev/null | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"
done
📝 Notes
✅ Works with .gz, .zip, .7z, .tar.gz, and other 7z-supported formats.
✅ Stops when there are no more compressed files.
✅ Can be modified to support additional compression tools.
