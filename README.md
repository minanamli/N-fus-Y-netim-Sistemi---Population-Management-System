# N-fus-Y-netim-Sistemi---Population-Management-System
import json

nufus = {}
try:
    with open("kisiler.json", "r", encoding="utf-8") as f:
        nufus = json.loads(f.read())

    print(f"Nufus sisteminde anlik olarak {len(nufus)} kisi bulunmaktadir.\n")
except FileNotFoundError:
    print("Nufus sisteminde kimse bulunmamaktadir. Yeni kayit olusturarak veri girisi yapabilirsiniz.\n")

    pass


class Kisi:
    def __init__(self, tc):
        self.tc = tc
        self.bilgileri_al()

    def bilgileri_al(self):
        self.ad = Kisi.get_text("Ad")
        self.soyad = Kisi.get_text("Soyad")
        self.baba = Kisi.get_text("Baba Adi")
        self.anne = Kisi.get_text("Anne Adi")
        self.dogum = Kisi.get_text("Dogum Yeri")
        self.durum = Kisi.get_text("Medeni Durum")
        self.kan = Kisi.get_kan()
        self.k_sehir = Kisi.get_text("Kutuk Sehir")
        self.k_ilce = Kisi.get_text("Kutuk Ilce")
        self.i_sehir = Kisi.get_text("Ikamet Sehir")
        self.i_ilce = Kisi.get_text("Ikamet Ilce")

    @staticmethod
    def get_tc():
        tc = input("TC No: ")
        while True:
            if len(tc) == 11 and tc.isdigit():
                return tc
            else:
                print("TC Girisi Hatali! Tamami rakamlardan olusmali ve 11 haneli olmalidir.")
                tc = input("TC No: ")

    @staticmethod
    def get_text(alan):
        text = input(f"{alan.capitalize()}: ")
        while True:
            if text.isalpha():
                return text
            else:
                print(f"{alan} Girisi Hatali!")
                text = input(f"{alan.capitalize()}: ")

    @staticmethod
    def get_kan():
        kan = input(f"Kan Grubu (A +, AB -): ")
        while True:
            if len(kan.split()) == 2:
                grup, rh = kan.split()
                if grup.upper() in ["0", "A", "B", "AB"] and rh in ["+", "-"]:
                    return f"{grup.upper()} RH {rh}"

            print(f"Kan Grubu Girisi Hatali!")
            kan = input(f"Kan Grubu (A +, AB -): ")

    def json(self):
        return {
            self.tc: {
                "adi": self.ad,
                "soyadi": self.soyad,
                "baba adi": self.baba,
                "anne adi": self.anne,
                "dogum yeri": self.dogum,
                "medeni durum": self.durum,
                "kan grubu": self.kan,
                "kutuk sehir": self.k_sehir,
                "kutuk ilce": self.k_ilce,
                "ikametgah sehir": self.i_sehir,
                "ikametgah ilce": self.i_ilce,
            }
        }


def yeni_kisi():
    kisi = Kisi.get_tc()
    if kisi in nufus.keys():
        print("Bu kisi zaten listede kayitlidir.")
    else:
        yeni = Kisi(kisi)
        nufus.update(yeni.json())


def arama_yap():
    kisi = Kisi.get_tc()
    if kisi in nufus.keys():
        print("Aradiginiz kisi listede mevcuttur. Kisiye ait bilgiler:")
        print(f"TC: {kisi}")
        for k, v in nufus[kisi]:
            print(f"{k}: {v}")

    else:
        print("Aradiginiz kisi bulunamamistir.")


def kontrol():
    secim = input("Baska bir bilgi guncellemek istiyor musun? (e/h): ")
    while True:
        if secim == "e":
            return True
        elif secim == "h":
            return False
        else:
            print("Hatali Secim!")
            secim = input("Baska bir bilgi guncellemek istiyor musun? (e/h): ")


def kisi_guncelle():
    kisi = Kisi.get_tc()
    if kisi not in nufus.keys():
        print("Aradiginiz kisi bulunamamistir.")
        return

    while True:
        print(
            "***********************************************\n"
            "| [1]Ad                 | [7]Kan Grubu        |\n"
            "| [2]Soyad              | [8]Kutuk Sehir      |\n"
            "| [3]Baba Adi           | [9]Kutuk Ilce       |\n"
            "| [4]Anne Adi           | [10]Ikametgah Sehir |\n"
            "| [5]Dogum Yeri         | [11]Ikametgah Ilce  |\n"
            "| [6]Medeni Durum       | [0]Cikis Yap        |\n"
            "***********************************************\n"
        )
        secim = input("Guncellemek istediginiz bilgiyi seciniz: ")
        if secim == "1":
            ad = Kisi.get_text("Ad")
            nufus[kisi].update({"adi": ad})
        elif secim == "2":
            soyad = Kisi.get_text("Soyad")
            nufus[kisi].update({"soyadi": soyad})
        elif secim == "3":
            baba = Kisi.get_text("Baba Adi")
            nufus[kisi].update({"baba adi": baba})
        elif secim == "4":
            anne = Kisi.get_text("Anne Adi")
            nufus[kisi].update({"anne adi": anne})
        elif secim == "5":
            dogum = Kisi.get_text("Dogum Yeri")
            nufus[kisi].update({"dogum yeri": dogum})
        elif secim == "6":
            durum = Kisi.get_text("Medeni Durum")
            nufus[kisi].update({"medeni durum": durum})
        elif secim == "7":
            kan = Kisi.get_kan()
            nufus[kisi].update({"kan grubu": kan})
        elif secim == "8":
            k_sehir = Kisi.get_text("Kutuk Sehir")
            nufus[kisi].update({"kutuk sehir": k_sehir})
        elif secim == "9":
            k_ilce = Kisi.get_text("Kutuk Ilce")
            nufus[kisi].update({"kutuk ilce": k_ilce})
        elif secim == "10":
            i_sehir = Kisi.get_text("Ikamet Sehir")
            nufus[kisi].update({"ikametgah sehir": i_sehir})
        elif secim == "11":
            i_ilce = Kisi.get_text("Ikamet Ilce")
            nufus[kisi].update({"ikametgah ilce": i_ilce})
        elif secim == "0":
            return
        else:
            print("Hatali Secim!")

        if kontrol():
            continue
        else:
            return


def kisi_sil():
    kisi = Kisi.get_tc()
    if kisi not in nufus.keys():
        print("Aradiginiz kisi bulunamamistir.")
        return
    else:
        del nufus[kisi]


def listele():
    for k in nufus.keys():
        print(f"TC: {k} | Ad: {nufus[k]['adi']}")


while True:
    print(
        f"***********************\n",
        f"[1] Yeni Kisi Ekle\n",
        f"[2] Arama Yap (TC)\n",
        f"[3] Kisi Guncelle\n",
        f"[4] Kisi Sil\n",
        f"[5] Tum Kisileri Listele\n",
        f"[0] Cikis Yap\n",
        f"***********************\n",
    )

    secim = input("Seciminiz: ")
    if secim == "1":
        yeni_kisi()
    elif secim == "2":
        print("Aramak istediginiz kisini TC'sini giriniz. ", end="")
        arama_yap()
    elif secim == "3":
        print("Bilgilerini guncellemek istediginiz kisini TC'sini giriniz. ", end="")
        kisi_guncelle()
    elif secim == "4":
        print("Silmek istediginiz kisini TC'sini giriniz. ", end="")
        kisi_sil()
    elif secim == "5":
        listele()
    elif secim == "0":
        print("Cikis Yapiliyor..")
        with open("kisiler.json", "w", encoding="utf-8") as f:
            json.dump(nufus, f, indent=4, ensure_ascii=False)
        quit()
    else:
        print("Hatali Giris!")
