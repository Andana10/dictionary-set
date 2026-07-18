# Mengimpor modul regular expression (re) untuk mencocokkan pola string (seperti format ID)
import re

# Dataset simulasi berisi rekam medis pasien dalam bentuk list of dictionaries
medical_records = [
    {
        'patient_id': 'P1001',
        'age': 34,
        'gender': 'Female',
        'diagnosis': 'Hypertension',
        'medications': ['Lisinopril'],
        'last_visit_id': 'V2301',
    },
    {
        'patient_id': 'p1002',
        'age': 47,
        'gender': 'male',
        'diagnosis': 'Type 2 Diabetes',
        'medications': ['Metformin', 'Insulin'],
        'last_visit_id': 'v2302',
    },
    {
        'patient_id': 'P1003',
        'age': 29,
        'gender': 'female',
        'diagnosis': 'Asthma',
        'medications': ['Albuterol'],
        'last_visit_id': 'v2303',
    },
    {
        'patient_id': 'p1004',
        'age': 56,
        'gender': 'Male',
        'diagnosis': 'Chronic Back Pain',
        'medications': ['Ibuprofen', 'Physical Therapy'],
        'last_visit_id': 'V2304',
    }
]

# Fungsi untuk memeriksa apakah nilai-nilai di dalam satu record pasien memenuhi aturan (constraints) yang valid
def find_invalid_records(
    patient_id, age, gender, diagnosis, medications, last_visit_id
):
    # Menyusun kamus aturan/kriteria kevalidan data (menghasilkan nilai True atau False)
    constraints = {
        # patient_id harus bertipe string dan harus berformat 'p' diawali angka (tidak sensitif huruf besar/kecil)
        'patient_id': isinstance(patient_id, str)
        and re.fullmatch('p\d+', patient_id, re.IGNORECASE),
        
        # age harus bertipe integer dan minimal berumur 18 tahun
        'age': isinstance(age, int) and age >= 18,
        
        # gender harus bertipe string dan nilainya harus 'male' atau 'female' (dibuat huruf kecil dulu)
        'gender': isinstance(gender, str) and gender.lower() in ('male', 'female'),
        
        # diagnosis boleh bertipe string atau bernilai None (kosong)
        'diagnosis': isinstance(diagnosis, str) or diagnosis is None,
        
        # medications harus bertipe list dan SEMUA elemen di dalam list tersebut harus bertipe string
        'medications': isinstance(medications, list)
        and all([isinstance(i, str) for i in medications]),
        
        # last_visit_id harus bertipe string dan harus berformat 'v' diawali angka (tidak sensitif huruf besar/kecil)
        'last_visit_id': isinstance(last_visit_id, str)
        and re.fullmatch('v\d+', last_visit_id, re.IGNORECASE)
    }
    
    # Mengembalikan daftar (list) nama key yang nilainya tidak valid (False)
    return [key for key, value in constraints.items() if not value]

# Fungsi utama untuk memvalidasi struktur data dan nilai dari keseluruhan dataset
def validate(data):
    # Memeriksa apakah tipe data pembungkus adalah list atau tuple
    is_sequence = isinstance(data, (list, tuple))

    # Jika bukan list/tuple, tampilkan error dan hentikan proses (return False)
    if not is_sequence:
        print('Invalid format: expected a list or tuple.')
        return False
        
    # Flag penanda status, akan berubah jadi True jika ditemukan setidaknya satu kesalahan data
    is_invalid = False
    
    # Kumpulan (set) kunci yang wajib ada pada setiap dictionary rekam medis
    key_set = set(
        ['patient_id', 'age', 'gender', 'diagnosis', 'medications', 'last_visit_id']
    )

    # Melakukan perulangan untuk memeriksa data pasien satu per satu berdasarkan indeksnya
    for index, dictionary in enumerate(data):
        
        # Validasi 1: Memastikan elemen di dalam list berupa tipe data dictionary
        if not isinstance(dictionary, dict):
            print(f'Invalid format: expected a dictionary at position {index}.')
            is_invalid = True
            continue # Lanjut ke data berikutnya

        # Validasi 2: Memastikan kunci (keys) di dictionary pas/sesuai dengan kriteria key_set
        if set(dictionary.keys()) != key_set:
            print(
                f'Invalid format: {dictionary} at position {index} has missing and/or invalid keys.'
            )
            is_invalid = True
            continue # Lanjut ke data berikutnya

        # Validasi 3: Memeriksa isi nilai dari data menggunakan fungsi find_invalid_records
        # **dictionary digunakan untuk membongkar key-value menjadi argumen fungsi
        invalid_records = find_invalid_records(**dictionary)
        
        # Perulangan untuk mencetak setiap key yang tidak lolos validasi aturan (constraints)
        for key in invalid_records:
            print(f"Unexpected format '{key}: {dictionary[key]}' at position {index}.")
            is_invalid = True
            
    # Jika selama proses perulangan ditemukan data cacat, fungsi mengembalikan False
    if is_invalid:
        return False
        
    # Jika semua data lolos pemeriksaan struktur dan nilai, tampilkan pesan sukses
    print('Valid format.')
    return True

# Menjalankan fungsi validasi pada data medical_records yang sudah disiapkan di atas
validate(medical_records)
