# RAG Evaluation & Chunking Strategy Lab 🚀

Bu proje; bir **Retrieval-Augmented Generation (RAG)** sisteminin başarımını uçtan uca ölçmek, farklı metin parçalama (chunking) stratejilerinin arama kalitesine etkisini analiz etmek ve LLM tabanlı hakem (LLM-as-judge) mekanizmalarıyla yanıt sadakatini (Faithfulness) denetlemek amacıyla geliştirilmiştir.

---

## 📌 Proje Özeti ve Kazanımlar

RAG mimarilerinde sadece doğru bilgiyi getirmek (Retrieval) yetmez; üretilen yanıtın bağlama sadık kalması (Generation/Faithfulness) da kritiktir. Bu çalışmada iki ana odak noktası üzerinde durulmuştur:

1. **Ölçüm Metrikleri (Evaluation):**
   * **Hit-Rate@k:** Arama motorunun (FAISS) istenen bilgiyi ilk $k$ sonuç arasında getirme başarısı.
   * **Faithfulness (Sadakat/Bağlılık):** Yerel dil modeli (`TinyLlama-1.1B`) hakem olarak kullanılarak, üretilen yanıtların sunulan bağlama sadık kalıp kalmadığı veya halüsinasyon içerip içermediği denetlenmiştir.

2. **Metin Parçalama Stratejileri (Chunking):**
   * **Strateji A:** Sabit Kelime Penceresi + Örtüşme (Fixed Window + Overlap)
   * **Strateji B:** Paragraf Bazlı Bölme (Semantic/Paragraph Boundaries)

---

## 📊 Öne Çıkan Deney Sonuçları

### 1. Chunking Stratejilerinin Hit-Rate@1 Karşılaştırması
Gerçek teknik staj notları üzerindeki arama performansında paragrafların anlamsal bütünlüğünün korunması başarıyı doğrudan artırmıştır:

| Strateji | Parça (Chunk) Sayısı | Hit-Rate@1 |
| :--- | :---: | :---: |
| **Strateji A (Fixed Window: 20 kelime, 5 overlap)** | 6 | **%66.7** |
| **Strateji B (Paragraf Bazlı Bölme)** | 3 | **%100.0** |

* **Bulgu:** Sabit kelime pencereleri anlamsal bağlamı cümle ortasından kesebildiği için Hit-Rate düşmektedir. Paragraf bazlı parçalama, teknik içeriklerde anlamsal bütünlüğü koruduğu için %100 başarı sağlamıştır.

### 2. Overlap (Örtüşme) Mekanizması
* `overlap=0` yapıldığında tanım ve çözüm cümleleri farklı chunk'lara bölünerek bilgi kaybına yol açmıştır.
* `overlap=5` eklendiğinde iki parça arasında anlamsal bir köprü kurulmuş ve sınırda kalan bilgilerin kaybolması engellenmiştir.

### 3. Faithfulness (LLM-as-Judge) ve Sınırları
* Model tarafından üretilen yanıtlar hakem fonksiyondan geçirilerek denetlenmiştir.
* **Sınır:** Küçük parametreli modeller (1.1B) bazen alakasız bağlamlarda uydurulan bilgileri gözden kaçırabilmektedir (`EVET` onayı verebilmektedir). Bu durum, RAG denetiminde daha büyük boyutlu LLM'lerin (Llama-3-70B, GPT-4o vb.) veya çoklu örnekli (few-shot) yönlendirmelerin önemini canlı olarak kanıtlamıştır.

---

## 🛠️ Kullanılan Teknolojiler ve Kütüphaneler

* **Language:** Python 3.10+
* **Vector Index:** `faiss-cpu` (IndexFlatIP - L2 Normalized Inner Product)
* **Embeddings:** `sentence-transformers` (`all-MiniLM-L6-v2`)
* **LLM / Generation:** `transformers` (`TinyLlama/TinyLlama-1.1B-Chat-v1.0`)

---

## 🚀 Projeyi Çalıştırma

1. Repozitörü klonlayın:
   ```bash
   git clone [https://github.com/ElifBerra/rag-evaluation-and-chunking-lab.git](https://github.com/kullanici_adi/rag-evaluation-and-chunking-lab.git)
   cd rag-evaluation-and-chunking-lab
   ```
2. Gerekli kütüphaneleri yükleyin:
   ```bash
   pip install torch transformers sentence-transformers faiss-cpu
   ```
3. staj_notlari.txt dosyasını ana dizine ekleyip Jupyter Notebook'u başlatın:
   ```bash
   jupyter notebook rag_evaluation_and_chunking.ipynb
   ```
