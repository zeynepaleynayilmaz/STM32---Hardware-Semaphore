# STM32 Dual-Core HSEM Demo

Bu proje, STM32WL55 çift çekirdekli (Cortex-M4 & Cortex-M0+) yapıda Hardware Semaphore (HSEM) kullanarak çekirdekler arası senkronizasyonu gösteren örnek bir uygulamadır. Proje STM32CubeIDE üzerinde oluşturulmuş olup hem çekirdek ayrımı hem de HSEM mekanizmasının kullanımını içermektedir.

### 🚀 Projenin Amacı

Bu projenin amacı, iki çekirdeğin ortak bir kaynağa (örn. USART2, global değişken, kritik fonksiyon) erişimini HSEM ile güvenli biçimde senkronize etmektir.

### 🧠 Hardware Semaphore (HSEM) Yapısı

STM32WL serisinde 16 adet donanımsal semaphore bulunur.
Her semaphore şu bilgileri tutar:

Alan	Açıklama
CoreID	Semaforu kilitleyen çekirdek: M4=4, M0+=8
ProcessID	Kullanıcı tarafından belirlenen 8-bit işlem/iş parçacığı ID'si
Lock	1: Kilitli, 0: Boş
Her çekirdek kendi main.c dosyasından HSEM kullanımını başlatır.

### 🔄 Çekirdekler Arası İletişim Akışı

M4, HSEM#0 için HAL_HSEM_FastTake(0) çağırır

HSEM boşsa otomatik kilitlenir

M4, USART2’ye veri yazar

M4 kilidi HAL_HSEM_Release(0, 0) ile bırakır

M0+, HSEM#0’ın boşaldığını interrupt üzerinden görür

M0+ semaforu alıp kendi işlemini yapar

Bu mekanizma yarış durumlarını (race condition) engeller.

