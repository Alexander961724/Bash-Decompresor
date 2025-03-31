# Bash-Decompresor
This Bash script recursively extracts nested compressed files using **7z** until no more archives remain.
🛠️ How the Script Works
This script follows a recursive extraction method:

🔹 Extracts first_file_name using 7z.
🔹 Identifies if the extracted file is another compressed archive.
🔹 If it is, extracts that file too.
🔹 Continues until no more compressed files remain.
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
decompresed_file_name="$(7z l data.gz | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"
7z x $first_file_name &>/dev/null

while [ $decompresed_file_name ]; do
    echo -e "\n${yellowColour}[+]${endColour} New extracted file: $decompresed_file_name"
    7z x $decompresed_file_name &>/dev/null
    decompresed_file_name="$(7z l $decompresed_file_name 2>/dev/null | tail -n 3 | head -n 1 | awk 'NF{print $NF}')"
done
#📝 Notes
✅ Works with .gz, .zip, .7z, .tar.gz, and other 7z-supported formats.
✅ If an extracted file is not compressed, the script stops.
✅ You can modify the script to support other compression tools.
