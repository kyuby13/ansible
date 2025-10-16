# 🔍 Ansible Ping Check

Proyek ini digunakan untuk melakukan pengecekan konektivitas (ping) dari satu host ke sekumpulan IP berdasarkan environment dan cluster tertentu menggunakan Ansible.

## 📁 Struktur Direktori
ping-host-to-host
├── README.md
├── inventory
│   ├── digid
│   │   ├── group_vars
│   │   │   └── all.yml     ## berisi semua variable target_ips_x.txt tinggal dipilih dengan comment uncomment yang ingin digunakan
│   │   └── inventory.ini   ## berisi semua list ip sumber dari digid
│   └── qoin
│       ├── group_vars
│       │   └── all.yml     ## berisi semua variable target_ips_x.txt tinggal dipilih dengan comment uncomment yang ingin digunakan
│       └── inventory.ini   ## berisi semua list ip sumber dari env qoin
├── roles
│   └── ping-check          
│       ├── files
│       │   ├── target_ips_dev.txt          ## targetip atau tujuan namafile harus sama dengan yang ada di group vars all.yml
│       │   ├── target_ips_gen.txt
│       │   ├── target_ips_internet.txt
│       │   ├── target_ips_man.txt
│       │   ├── target_ips_prod.txt
│       │   ├── target_ips_produe.txt
│       │   ├── target_ips_sand.txt
│       │   └── target_ips_sta.txt
│       └── tasks
│           └── main.yml                ## Logic utama pengecekan ping
└── site.yml                            ## Playbook utama


⚙️ Cara Kerja
File IP Target:
    - Nama file IP disesuaikan dengan kombinasi env + cluster, misal: target_ips_dev_qoin.txt.
    - File ini dibaca melalui variable target_ip_file di dalam group_vars/all.yml.
Task Utama:
    - Task akan membaca file IP target, melakukan ping satu per satu, lalu menampilkan hasil yang sukses dan gagal.

🚀 Cara Menjalankan
1. masuk kefolder root, disini contoh ada di /home/hendro/ansible/ping-host-to-host
cd /home/hendro/ansible/ping-host-to-host

2. comment/uncomment tujuan ip ada di inventory/qoin/group_vars/all.yml contoh di sini target_ips_internet.txt 
target_ip_file: "target_ips_dev.txt"
# target_ip_file: "target_ips_sta.txt"
# target_ip_file: "target_ips_prod.txt"
# target_ip_file: "target_ips_produe.txt"
# target_ip_file: "target_ips_man.txt"
# target_ip_file: "target_ips_sand.txt"
# target_ip_file: "target_ips_internet.txt"
# target_ip_file: "target_ips_gen.txt"

3. Jalankan playbook contoh disini ping dari qoin_man ke dev
ansible-playbook -i inventory/qoin/inventory.ini site.yml -l qoin_man

📌 Catatan
Variabel target_ip_file harus sesuai dengan nama file yang tersedia di folder roles/ping-check/files.
Penamaan environment dan cluster berpengaruh terhadap path file dan konfigurasi.

4. Jangan lupa setting juga ansible host di /etc/ansible/hosts untuk qoin_man nya
