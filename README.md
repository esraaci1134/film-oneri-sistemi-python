
# 🎬 FILMPERSONA: MBTI & Estetik Tercih Odaklı Akıllı Film Öneri Platformu

[![Python](https://img.shields.io/badge/Python-3.9+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![Streamlit](https://img.shields.io/badge/Streamlit-FF4B4B?style=for-the-badge&logo=Streamlit&logoColor=white)](https://streamlit.io/)
[![Firebase](https://img.shields.io/badge/Firebase_Firestore-FFCA28?style=for-the-badge&logo=firebase&logoColor=black)](https://firebase.google.com/)
[![TMDB API](https://img.shields.io/badge/TMDB_API-01B4E4?style=for-the-badge&logo=themoviedb&logoColor=white)](https://www.themoviedb.org/)
[![Live App](https://img.shields.io/badge/Live_Demo-Streamlit_Cloud-00C7B7?style=for-the-badge&logo=streamlit&logoColor=white)](https://filmpersona.streamlit.app/)

**FILMPERSONA**, dijital yayın platformlarında yaygın olarak karşılaşılan **"Soğuk Başlangıç" (Cold Start)** problemini aşmak amacıyla geliştirilmiş, bilişsel psikoloji (MBTI) ve sinematik estetik tercihlerini temel alan bulut tabanlı bir karar destek ve film öneri sistemidir.

---

## 🚀 Öne Çıkan Özellikler

* **🧠 Bilişsel Psikometrik Eşleşme (MBTI):** Kullanıcının 4 ana eksendeki (E/I, S/N, T/F, J/P) bilişsel haritasını 16 kişilik tipinden birine bağlayarak arama uzayını dinamik olarak filtreler.
* **⚡ Soğuk Başlangıç Çözümü:** Kullanıcının geçmiş izleme verilerine ihtiyaç duymadan, ilk anda nokta atışı içerik önerileri sunar.
* **☁️ Bulut NoSQL Veri Tabanı:** Kullanıcı profilleri, tercih matrisleri ve süzülmüş film havuzları **Google Firebase Cloud Firestore** üzerinde gerçek zamanlı ve ilişkisel olmayan (NoSQL) döküman yapısıyla saklanır.
* **🌐 Asenkron On-Demand Çeviri:** API kota kullanımını %90 oranında düşürmek amacıyla film detayları ve özetleri yalnızca kart etkileşimi anında (`@st.dialog`) **Google Translator API** vasıtasıyla Türkçe'ye çevrilir.
* **🎬 Akıllı Fragman Önceliklendirme (Priority Queue):** YouTube üzerinden Türkçe Altyazı $\rightarrow$ Türkçe Dublaj $\rightarrow$ Orijinal Fragman $\rightarrow$ Dinamik Arama Yedeği sıralamasıyla otomatik oynatımlı (`autoplay=1`) fragman akışı sağlar.
* **🔒 Kriptografik Oturum Yönetimi:** Kullanıcı şifreleri `hashlib.sha256` algoritması ile şifrelenerek veri tabanında güvenli bir şekilde saklanır.

---

## 🏗️ Sistem Mimarisi (Three-Tier Architecture)

Sistem, sürdürülebilir ve modüler bir mimari için 3 katmanlı yapıda tasarlanmıştır:

```
[Sunum Katmanı (UI)]      <--->   [Mantıksal Katman (Engine)]   <--->   [Veri Katmanı (Cloud NoSQL)]
Streamlit Framework              Python Engine & pandas                 Firebase Cloud Firestore
(Custom CSS Grid & Dialogs)      (Filtering & Priority Queue)          (Real-time JSON Documents)
```

---

## 🛠️ Teknik Araçlar ve Kütüphaneler

| Katman / Bileşen | Kullanılan Teknoloji / Kütüphane | Kullanım Amacı |
| :--- | :--- | :--- |
| **Ön Yüz (UI)** | `Streamlit`, Custom CSS | Responsive 4'lü grid katalog düzeni ve pop-up arayüzleri |
| **Veri İşleme** | `pandas`, `ast.literal_eval` | TMDB ham veri temizliği, JSON tür ayrıştırması |
| **Bulut Veri Tabanı** | `firebase-admin`, Cloud Firestore | Profil saklama, asenkron sorgular (`where().stream()`) |
| **API & Mikro Servisler** | TMDB REST API, YouTube Video API | Afiş çekimi, dinamik fragman arama ve medya eşleme |
| **Çeviri Motoru** | `deep-translator` | Asenkron İngilizce-Türkçe özet çevirisi |
| **Güvenlik** | `hashlib` (SHA-256) | Kullanıcı kimlik doğrulama ve şifre hash'leme |

---

## 🔄 Veri Ön İşleme ve Filtreleme Algoritması

1. **Veri Temizleme:** Kaggle TMDB 5000 veri seti (`tmdb5000movies.csv`) Pandas ile işlenmiş; IMDb puanı 6.0'ın altında (`vote_average < 6.0`) olan içerikler veri setinden elenmiştir.
2. **Çift Katmanlı Süzgeç:**
   * **Bilişsel Kara Liste:** Kişilik tipinin psikolojik olarak kaçındığı türler (örn. ISTJ için Korku/Gore) sorguya gitmeden elenir.
   * **Bilişsel Beyaz Liste & Estetik:** En uyumlu türler kilitlenir; 10 soruluk sinematik estetik tercihlerine göre (örn: "Neon Işıklar" seçimiyle *Bilim Kurgu*) öneri rafı dinamik olarak genişletilir.

---

## 📁 Proje Dosya Yapısı

```
FILMPERSONA/
├── arayuz.py                 # Streamlit ön yüz tasarımı ve API sorgu yönetimi
├── hazirlik.py               # MBTI skorlama motoru ve Firestore bağlantı fonksiyonları
├── serviceAccountKey.json    # Firebase Cloud güvenlik sertifikası (Local dev)
├── requirements.txt          # Proje bağımlılıkları
└── README.md                 # Proje dokümantasyonu
└── tmdb_5000_movies.csv      # Kullanılan veri seti

---

## ⚙️ Yerel Kurulum ve Çalıştırma

Projeyi kendi yerel ortamınızda çalıştırmak için aşağıdaki adımları takip edebilirsiniz:

1. **Depoyu klonlayın:**
   ```bash
   git clone https://github.com/esraaci/F-LMPERSONA.git
   cd F-LMPERSONA
   ```

2. **Gerekli bağımlılıkları yükleyin:**
   ```bash
   pip install -r requirements.txt
   ```

3. **Firebase Sertifikasını Ekleyin:**
   Firebase konsolunuzdan indireceğiniz `serviceAccountKey.json` dosyasını ana dizine ekleyin veya Streamlit secrets yapısını yapılandırın.

4. **Uygulamayı başlatın:**
   ```bash
   streamlit run arayuz.py
   ```

---

## 🌐 Canlı Demo & Bağlantılar

* **Canlı Uygulama Demosu:** [filmpersona.streamlit.app](https://filmpersona.streamlit.app/)
* **Geliştirici:** Esra ACI
