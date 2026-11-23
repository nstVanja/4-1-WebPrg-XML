# XML i ASP.NET – materijal za učenike

Nov 23, 2026&#32;

## O lekciji

Za šest časova naučićeš da zapišeš podatke u XML formatu i da ih iz ASP.NET stranice čitaš, dopunjuješ i brišeš. Lekcija ima dva bloka po tri časa: prvi je o samom XML-u, drugi o radu sa XML fajlom u web aplikaciji.

| Blok | Čas | Tema | Vežbe |
| --- | --- | --- | --- |
| 1 | 1 | Šta je XML, struktura dokumenta, stablo | 1 |
| 1 | 2 | Pravila dobro formiranog dokumenta | 2, 3 |
| 1 | 3 | Element ili atribut, validnost, XML i JSON | 4, 5 |
| 2 | 4 | Čitanje XML fajla i prikaz u GridView-u | B1 |
| 2 | 5 | Dodavanje u XML fajl, poruke i provera unosa | B2 |
| 2 | 6 | Brisanje i pretraga | B3, B4, B5 |

Posle lekcije treba da umeš da:

- objasniš šta je XML i čemu služi, i po čemu se razlikuje od HTML-a;
- napišeš dobro formiran XML dokument i pronađeš greške u tuđem;
- razlikuješ element, atribut i vrednost i izabereš šta će biti atribut;
- učitaš XML fajl u DataSet i prikažeš ga u GridView-u;
- dodaš i obrišeš podatke u XML fajlu iz ASP.NET stranice i pretražiš ih.

Radimo u Visual Studiju 2010 (ASP.NET Web Site, C#). XML fajl možeš da otvoriš i u pregledaču (Chrome, Firefox) da proveriš da li je ispravan.

## Blok 1 · Čas 1 – Šta je XML

XML (eXtensible Markup Language, proširivi jezik za označavanje) je tekstualni format za zapisivanje i razmenu podataka. HTML opisuje kako stranica izgleda, a XML šta podaci znače. „Proširiv“ znači da oznake (tagove) izmišljaš sam: `<knjiga>`, `<autor>`, `<cena>`.

### Gde se koristi

- **web.config** u tvom ASP.NET projektu je XML fajl.
- **Office dokumenti**: .docx i .xlsx su ZIP arhive pune XML fajlova. Probaj: kopiraj neki .docx, promeni mu nastavak u .zip i otvori ga – tekst je u fajlu word/document.xml.
- **Razmena podataka** između programa pisanih u različitim jezicima, na primer elektronske fakture koje firme šalju jedna drugoj.
- **SVG slike**, RSS vesti, rasporedi ekrana u Android aplikacijama.
- **DataSet** u .NET-u ume da se zapiše u XML i da se učita iz njega. To radimo u bloku 2.

### Struktura XML dokumenta

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ucenik>
    <ime>Marko</ime>
    <prezime>Marković</prezime>
    <razred>4</razred>
</ucenik>
```

| Deo | Primer | Značenje |
| --- | --- | --- |
| Deklaracija | `<?xml version="1.0" encoding="UTF-8"?>` | prvi red: verzija i kodiranje; UTF-8 zbog slova č, ć, ž, š, đ; preporučuje se |
| Koreni element | `<ucenik>` | obuhvata sve ostale elemente; u dokumentu je samo jedan |
| Podređeni elementi | `<ime>`, `<prezime>`, `<razred>` | delovi podatka, ugnežđeni u koreni |
| Vrednost | `Marko` | tekst između početnog i završnog taga |

Element = početni tag + sadržaj + završni tag, na primer `<ime>Marko</ime>`. Završni tag počinje kosom crtom.

### Elementi, atributi i vrednosti

Isti proizvod može da se zapiše na dva načina. Prvi put su naziv i cena elementi:

```xml
<proizvod>
    <naziv>Miš</naziv>
    <cena>1500</cena>
</proizvod>
```

Drugi put su atributi – parovi `ime="vrednost"` unutar početnog taga:

```xml
<proizvod naziv="Miš" cena="1500" />
```

Element bez sadržaja može da se zatvori odmah, sa `/>` na kraju. To je **prazan element**. Šta će biti atribut, a šta element, odlučuje autor dokumenta; pravilo palca učimo na času 3.

### XML je stablo

Elementi su ugnežđeni jedan u drugi, pa dokument liči na porodično stablo: koren na vrhu, ispod njega deca, pa unuci. Elementi sa istim roditeljem su braća.

```xml
<skola>
    <ucenici>
        <ucenik id="1">
            <ime>Jelena</ime>
            <razred>4</razred>
        </ucenik>
        <ucenik id="2">
            <ime>Stefan</ime>
            <razred>4</razred>
        </ucenik>
    </ucenici>
</skola>
```

&#91;embedded content: stablo dokumenta skola · 4 nivoa\]

Atribut `id` nije posebna grana stabla: on pripada elementu `ucenik` u čijem je početnom tagu.

### XML i HTML

|  | XML | HTML |
| --- | --- | --- |
| Namena | opisuje podatke | opisuje izgled stranice |
| Tagovi | izmišlja ih autor | unapred zadati: `<p>`, `<table>`, `<img>` |
| Greška u dokumentu | dokument se ne učita | pregledač preskoči grešku i prikaže šta može |
| Velika i mala slova | razlikuju se | ne razlikuju se |
| Upotreba | čuvanje i razmena podataka | web stranice |

### Kako napraviti XML fajl u Visual Studiju 2010

1. File → New File → XML File (u projektu: desni klik na projekat → Add New Item → XML File).
2. VS sam upiše deklaraciju. Kad otkucaš `>` na kraju početnog taga, VS dopiše završni tag.
3. Sačuvaj fajl (Ctrl+S) i otvori ga u pregledaču: desni klik → View in Browser, ili prevuci fajl u Chrome. Ispravan XML se prikaže kao stablo koje možeš da sklapaš i otvaraš.

Sada uradi **Vežbu 1**.

## Blok 1 · Čas 2 – Pravila dobro formiranog XML-a

Dokument koji poštuje svih šest pravila zove se **dobro formiran** (well-formed). Ako prekrši ijedno, program koji ga čita prijavi grešku i ne učita ništa. HTML je blaži: pregledač preskoči grešku, XML ne prašta.

### Šest pravila

| # | Pravilo | Pogrešno | Ispravno |
| --- | --- | --- | --- |
| 1 | Tačno jedan koreni element | `<a>1</a><b>2</b>` | `<lista><a>1</a><b>2</b></lista>` |
| 2 | Svaki element se zatvara | `<ime>Ana` | `<ime>Ana</ime>` ili `<prazan />` |
| 3 | Tagovi se ne ukrštaju | `<b><i>tekst</b></i>` | `<b><i>tekst</i></b>` |
| 4 | Velika i mala slova se razlikuju | `<Ime>Ana</ime>` | `<ime>Ana</ime>` |
| 5 | Vrednost atributa je u navodnicima | `<ucenik id=7>` | `<ucenik id="7">` |
| 6 | Znakovi `<` i `&` ne pišu se direktno u tekstu | `<firma>Petrović & sinovi</firma>` | `<firma>Petrović &amp; sinovi</firma>` |

### Imena elemenata i atributa

- Počinju slovom ili donjom crtom, ne cifrom: `<1razred>` ne može, `<razred1>` može.
- Nemaju razmak: umesto `<datum rodjenja>` piši `<datumRodjenja>` ili `<datum_rodjenja>`.
- Ne počinju slovima xml (u bilo kojoj kombinaciji velikih i malih slova), jer su ta imena rezervisana.
- Crtica i tačka smeju u sredini imena. Slova č, ć, š, ž, đ su dozvoljena, ali ih izbegavamo, jer imena kasnije koristimo u C# kodu: `<godina>`, `<reziser>`, `<zanr>`.

### Specijalni znakovi

Neki znakovi imaju posebno značenje u XML-u, pa se u tekstu pišu kao **entiteti**:

| Znak | Entitet | Kada je obavezan |
| --- | --- | --- |
| `<` | `&lt;` | uvek u tekstu i u vrednosti atributa |
| `&` | `&amp;` | uvek u tekstu i u vrednosti atributa |
| `>` | `&gt;` | nije obavezan, ali se preporučuje |
| `"` | `&quot;` | u vrednosti atributa koja je pod dvostrukim navodnicima |
| `'` | `&apos;` | u vrednosti atributa koja je pod jednostrukim navodnicima |

Primer: `<uslov>a &lt; b</uslov>`. Program koji čita dokument dobija tekst a < b.

Ako tekst ima mnogo specijalnih znakova, na primer deo koda, može da se stavi u CDATA sekciju. Sve unutar nje je običan tekst: `<kod><![CDATA[if (a < b && c > 0)]]></kod>`.

### Komentari

```xml
<!-- Ovo je komentar. Program koji čita XML ga preskoči. -->
<knjiga>
    <naslov>Prokleta avlija</naslov>
</knjiga>
```

Komentar ne može da stoji unutar taga i ne sme da sadrži dve crtice zaredom.

### Kako proveriti dokument

- **Visual Studio 2010**: greška je podvučena talasastom linijom, a opis sa brojem reda je u prozoru Error List (View → Error List).
- **Pregledač**: za neispravan XML Chrome i Firefox umesto stabla prikažu poruku o grešci sa brojem reda i kolone.
- Program staje na **prvoj** grešci. Ispravi je, sačuvaj, osveži stranicu i traži sledeću.

Sada uradi **Vežbe 2 i 3**.

## Blok 1 · Čas 3 – Element ili atribut, validnost, XML i JSON

### Element ili atribut?

Strogog pravila nema, ali ovo pravilo palca radi u većini slučajeva:

| Atribut | Element |
| --- | --- |
| oznaka ili identifikator: `id="7"`, `jezik="sr"`, `valuta="RSD"` | podatak koji čitamo i menjamo: naslov, autor, opis |
| jedna kratka vrednost | vrednost koja može da se ponovi (više žanrova) |
|  | vrednost koja ima delove (adresa: ulica, broj, grad) |

```xml
<knjiga id="12">
    <naslov>Prokleta avlija</naslov>
    <autor>Ivo Andrić</autor>
    <cena valuta="RSD">890</cena>
</knjiga>
```

### Ponovljeni i složeni podaci

Isti atribut ne sme da se pojavi dva puta u jednom tagu. Kad film ima više žanrova, pravimo element-omotač sa više istoimenih elemenata:

```xml
<film>
    <naslov>Ko to tamo peva</naslov>
    <zanrovi>
        <zanr>komedija</zanr>
        <zanr>drama</zanr>
    </zanrovi>
</film>
```

Podatak koji ima delove takođe postaje element sa podelementima:

```xml
<adresa>
    <ulica>Glavna</ulica>
    <broj>12</broj>
    <grad>Kula</grad>
</adresa>
```

### Dobro formiran i validan

- **Dobro formiran** dokument poštuje šest pravila sa časa 2.
- **Validan** dokument je dobro formiran i uz to odgovara šemi – opisu koji kaže koji elementi postoje, kojim redom, koliko puta i kog su tipa. Šema se piše kao DTD ili kao XSD (XML Schema).

Svaki validan dokument je dobro formiran, ali obrnuto ne važi. Ovaj dokument je dobro formiran, ali nije validan po šemi ispod, jer godina nije ceo broj:

```xml
<knjiga>
    <naslov>Derviš i smrt</naslov>
    <autor>Meša Selimović</autor>
    <godina>hiljadu devetsto šezdeset šest</godina>
</knjiga>
```

Deo XSD šeme za knjigu (samo da vidiš kako izgleda, ne uči se napamet):

```xml
<xs:element name="knjiga">
  <xs:complexType>
    <xs:sequence>
      <xs:element name="naslov" type="xs:string" />
      <xs:element name="autor" type="xs:string" />
      <xs:element name="godina" type="xs:int" />
    </xs:sequence>
  </xs:complexType>
</xs:element>
```

### XML i JSON

JSON (JavaScript Object Notation) znaš iz JavaScript-a. Ista knjiga u oba formata:

```xml
<knjiga id="12">
    <naslov>Prokleta avlija</naslov>
    <autor>Ivo Andrić</autor>
    <godina>1954</godina>
</knjiga>
```

```json
{
    "id": 12,
    "naslov": "Prokleta avlija",
    "autor": "Ivo Andrić",
    "godina": 1954
}
```

|  | XML | JSON |
| --- | --- | --- |
| Zapis | početni i završni tag | parovi "ključ": vrednost |
| Dužina | duži, ime se ponavlja u završnom tagu | kraći |
| Tipovi podataka | sve je tekst; tip određuje šema | broj, tekst, true/false, niz, objekat |
| Atributi i komentari | ima | nema |
| Gde se sreće | konfiguracije, Office fajlovi, razmena između firmi, .NET | web servisi, JavaScript aplikacije |

Web servisi danas uglavnom šalju JSON, ali XML je i dalje svuda oko nas, a .NET ga čita u jednom redu koda. To koristimo u bloku 2.

Sada uradi **Vežbe 4 i 5**.

## Vežbe – blok 1

Svaki fajl praviš u Visual Studiju (File → New File → XML File) i proveriš ga u pregledaču. Fajlove čuvaj u svom folderu, trebaju ti u bloku 2.

### Vežba 1 – Prvi XML dokument

Napiši dokument biblioteka.xml sa tri knjige. Svaka knjiga ima naslov, autora, godinu izdanja i cenu.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<biblioteka>
    <knjiga>
        <naslov></naslov>
        <autor></autor>
        <godina></godina>
        <cena></cena>
    </knjiga>
</biblioteka>
```

### Vežba 2 – Elementi u atribute

Prepravi dokument iz Vežbe 1 tako da svi podaci budu atributi, i sačuvaj ga kao biblioteka2.xml:

```xml
<knjiga naslov="..." autor="..." godina="..." cena="..." />
```

U svesku zapiši: koji zapis je kraći, a koji pregledniji? Dodatno: napravi treću verziju u kojoj je samo `id` atribut, a ostalo elementi.

### Vežba 3 – Pronađi sedam grešaka

Prepiši dokument u fajl greske.xml i otvori ga u pregledaču. Ispravljaj grešku po grešku dok se ne prikaže stablo. Za svaku grešku zapiši u svesku broj reda, šta nije u redu i koje je pravilo prekršeno.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<ucenik id=7>
    <ime>Ana</ime>
    <prezime>Marković</prezime
    <razred>4</Razred>
    <smer>Tehničar informacionih tehnologija</smer>
    <broj>12
    <ocene><matematika>5</ocene></matematika>
    <sekcija>Web dizajn & programiranje</sekcija>
</ucenik>
<ucenik id="8"></ucenik>
```

### Vežba 4 – Katalog filmova

Napiši filmovi.xml sa pet filmova. Svaki film ima naslov, režisera, trajanje u minutima, godinu i jedan ili više žanrova.

```xml
<filmovi>
    <film>
        <naslov></naslov>
        <reziser></reziser>
        <trajanje></trajanje>
        <godina></godina>
        <zanrovi>
            <zanr></zanr>
            <zanr></zanr>
        </zanrovi>
    </film>
</filmovi>
```

### Vežba 5 – Adresar

Napiši adresar.xml sa najmanje pet kontakata. Svaki kontakt ima ime, prezime, telefon, e-mail i adresu (ulica, broj, grad).

```xml
<adresar>
    <kontakt>
        <ime></ime>
        <prezime></prezime>
        <telefon></telefon>
        <email></email>
        <adresa>
            <ulica></ulica>
            <broj></broj>
            <grad></grad>
        </adresa>
    </kontakt>
</adresar>
```

Dodatno: kontakt može da ima više telefona, a svaki telefon ima atribut tip (mobilni ili fiksni). Promeni strukturu tako da to podrži.

## Blok 2 · Čas 4 – Čitanje XML fajla u ASP.NET aplikaciji

DataSet čita ceo XML fajl jednom naredbom, `ReadXml`, i od njega napravi tabelu koju GridView prikaže. To je beskonekcioni pristup koji znaš iz rada sa bazom: podaci su u memoriji, samo što umesto `adapter.Fill(ds)` podatke daje fajl.

### Kako DataSet vidi XML

| U fajlu knjige.xml | U DataSet-u |
| --- | --- |
| koreni element `<biblioteka>` | sam DataSet |
| element koji se ponavlja, `<knjiga>` | tabela `ds.Tables[0]` |
| podelementi `<naslov>`, `<autor>`, `<godina>` | kolone tabele |
| svaka `<knjiga>` | jedan red (`DataRow`) |
| vrednost elementa | vrednost u ćeliji – uvek tekst (`string`) |

Kad elementi imaju svoje podelemente (adresa unutar kontakta), DataSet napravi više povezanih tabela. Zato u ovoj lekciji koristimo „ravne“ fajlove: koren, pa element koji se ponavlja, pa podaci.

### Korak po korak

1. File → New Web Site → ASP.NET Empty Web Site, jezik C#, ime XmlBiblioteka.
2. Desni klik na projekat → Add ASP.NET Folder → App\_Data.
3. Desni klik na App\_Data → Add New Item → XML File, ime knjige.xml. Upiši:

```xml
<?xml version="1.0" encoding="utf-8"?>
<biblioteka>
  <knjiga>
    <naslov>Na Drini ćuprija</naslov>
    <autor>Ivo Andrić</autor>
    <godina>1945</godina>
  </knjiga>
  <knjiga>
    <naslov>Alisa u zemlji čuda</naslov>
    <autor>Lewis Carroll</autor>
    <godina>1865</godina>
  </knjiga>
</biblioteka>
```

4. Desni klik na projekat → Add New Item → Web Form, ime Default.aspx (štiklirano Place code in separate file).
5. U Source prikazu između `<form>` i `</form>` upiši:

```html
<asp:Button ID="btnUcitaj" runat="server" Text="Učitaj XML" OnClick="btnUcitaj_Click" />
<br /><br />
<asp:GridView ID="GridView1" runat="server" CellPadding="10"></asp:GridView>
```

6. U Design prikazu dvoklik na dugme napravi metodu za obradu događaja `btnUcitaj_Click`. Ceo fajl Default.aspx.cs:

```csharp
using System;
using System.Data;   // DataSet, DataTable, DataRow

public partial class _Default : System.Web.UI.Page
{
    protected void btnUcitaj_Click(object sender, EventArgs e)
    {
        DataSet ds = new DataSet();
        string putanja = Server.MapPath("~/App_Data/knjige.xml");
        ds.ReadXml(putanja);

        GridView1.DataSource = ds.Tables[0];
        GridView1.DataBind();
    }
}
```

7. Pokreni sa Ctrl+F5 i klikni Učitaj XML.

### Šta radi koji red

| Red | Šta radi |
| --- | --- |
| `DataSet ds = new DataSet();` | pravi prazan DataSet u memoriji |
| `Server.MapPath("~/App_Data/knjige.xml")` | pretvara putanju u sajtu (`~` je koren sajta) u putanju na disku servera, npr. `C:\Sajtovi\XmlBiblioteka\App_Data\knjige.xml` |
| `ds.ReadXml(putanja);` | čita fajl i pravi tabelu sa kolonama i redovima |
| `GridView1.DataSource = ds.Tables[0];` | kaže GridView-u odakle su podaci |
| `GridView1.DataBind();` | GridView pravi HTML tabelu od podataka |

XML fajl stoji u App\_Data jer server fajlove iz tog foldera ne šalje pregledaču. Podaci se čitaju iz koda, ali ih niko ne može preuzeti kucanjem adrese.

### Probaj: tabela iz baze u XML

DataSet iz beskonekcionog pristupa ume i da se zapiše. U Upitniku, odmah posle `adapter.Fill(ds);`, dodaj red:

```csharp
ds.WriteXml(Server.MapPath("~/App_Data/izvoz.xml"));
```

Izvrši `SELECT Naslov, Autor FROM Knjige` nad bazom Biblioteka i otvori izvoz.xml (folder App\_Data napravi ako ga nema). Koreni element je `<NewDataSet>`, a svaki red iz baze je element `<Table>`.

Sada uradi **Vežbu B1**.

## Blok 2 · Čas 5 – Dodavanje knjige u XML fajl

Nova knjiga se prvo doda u tabelu u memoriji, a zatim `WriteXml` prepiše ceo fajl. Na kraju časa aplikacija ima i poruke o uspehu i proveru unosa, kao na slici:

&#91;image: Aplikacija Biblioteka: dugme Učitaj XML, tabela knjiga, polja za unos i brisanje sa zelenim porukama\]

### Polja za unos

U Default.aspx, ispod GridView-a, dodaj Label za poruku, tri TextBox-a i dugme:

```html
<hr />
<p><asp:Label ID="lblUnos" runat="server"></asp:Label></p>
<table>
    <tr>
        <td style="width:110px">Naslov:</td>
        <td><asp:TextBox ID="txtNaslov" runat="server" Width="250px"></asp:TextBox></td>
    </tr>
    <tr>
        <td>Autor:</td>
        <td><asp:TextBox ID="txtAutor" runat="server" Width="250px"></asp:TextBox></td>
    </tr>
    <tr>
        <td>Godina:</td>
        <td><asp:TextBox ID="txtGodina" runat="server" Width="250px"></asp:TextBox></td>
    </tr>
</table>
<br />
<asp:Button ID="btnDodaj" runat="server" Text="Dodaj knjigu" OnClick="btnDodaj_Click" />
```

### Četiri koraka dodavanja

```csharp
protected void btnDodaj_Click(object sender, EventArgs e)
{
    DataSet ds = new DataSet();
    string putanja = Server.MapPath("~/App_Data/knjige.xml");
    ds.ReadXml(putanja);                      // 1. učitaj

    DataRow novi = ds.Tables[0].NewRow();     // 2. prazan red sa kolonama tabele
    novi["naslov"] = txtNaslov.Text;
    novi["autor"] = txtAutor.Text;
    novi["godina"] = txtGodina.Text;
    ds.Tables[0].Rows.Add(novi);              // 3. red ulazi u tabelu

    ds.WriteXml(putanja);                     // 4. ceo DataSet nazad u fajl

    GridView1.DataSource = ds.Tables[0];
    GridView1.DataBind();
}
```

`NewRow` samo pravi red; red je u tabeli tek posle `Rows.Add`. Imena kolona u uglastim zagradama moraju biti ista kao imena elemenata u fajlu, uključujući mala slova.

### Šta se desi u fajlu

Posle prvog dodavanja otvori knjige.xml:

```xml
<?xml version="1.0" standalone="yes"?>
<biblioteka>
  <knjiga>
    <naslov>Na Drini ćuprija</naslov>
    <autor>Ivo Andrić</autor>
    <godina>1945</godina>
  </knjiga>
  ...
  <knjiga>
    <naslov>Ostrvo pelikana</naslov>
    <autor>Miodrag Majić</autor>
    <godina>2021</godina>
  </knjiga>
</biblioteka>
```

`WriteXml` prepisuje ceo fajl, pa se menja i deklaracija (dobija `standalone="yes"`). Ako je fajl otvoren u Visual Studiju, VS pita da li da ga ponovo učita – odgovori Yes.

### Poruke i provera unosa

Bez provere aplikacija upiše i prazna polja i godinu „abc“. Zato pre upisa proveravamo unos i u Label-u ispisujemo poruku: zelenu za uspeh, crvenu za grešku.

```csharp
string naslov = txtNaslov.Text.Trim();
string autor = txtAutor.Text.Trim();
int godina;

if (naslov == "" || autor == "")
{
    lblUnos.ForeColor = Color.Red;
    lblUnos.Text = "Unesite naslov i autora.";
    return;
}
if (!int.TryParse(txtGodina.Text.Trim(), out godina))
{
    lblUnos.ForeColor = Color.Red;
    lblUnos.Text = "Godina mora biti ceo broj.";
    return;
}
// ... četiri koraka dodavanja ...
lblUnos.ForeColor = Color.Green;
lblUnos.Text = "Uspešan unos";
```

- `Trim()` skida razmake sa početka i kraja, pa polje sa samo razmacima računamo kao prazno.
- `int.TryParse` vraća `true` ako tekst može da se pretvori u ceo broj i taj broj upiše u `godina`.
- `return` prekida metodu, pa se u fajl ništa ne upisuje.
- Za `Color` na vrh fajla dodaj `using System.Drawing;`.

Pogledaj treći red tabele na slici: naslov i autor su zamenjeni, a aplikacija je to prihvatila. Provera unosa može da proveri oblik podatka (polje nije prazno, godina je broj), ali ne i njegov smisao.

### Pomoćne metode

Ista tri reda (napravi DataSet, nađi putanju, učitaj) ponavljaju se u svakoj metodi za obradu događaja. Izdvojimo ih u pomoćne metode:

```csharp
private string Putanja()
{
    return Server.MapPath("~/App_Data/knjige.xml");
}

private DataSet UcitajKnjige()
{
    DataSet ds = new DataSet();
    ds.ReadXml(Putanja());
    return ds;
}

private void Prikazi(DataSet ds)
{
    GridView1.DataSource = ds.Tables[0];
    GridView1.DataBind();
}

protected void btnUcitaj_Click(object sender, EventArgs e)
{
    Prikazi(UcitajKnjige());
}
```

U `btnDodaj_Click` sada pišeš `DataSet ds = UcitajKnjige();`, `ds.WriteXml(Putanja());` i `Prikazi(ds);`. Ako se fajl preimenuje, menjaš jedan red umesto tri.

Sada uradi **Vežbu B2**.

## Blok 2 · Čas 6 – Brisanje i pretraga

Za brisanje tražimo redove metodom `Select`, brišemo ih sa `Delete` i ponovo zapisujemo fajl. Za pretragu koristimo `DataView`, koji GridView ume da prikaže.

### Brisanje po naslovu

U Default.aspx dodaj Label `lblBrisanje`, TextBox `txtObrisi` i dugme `btnObrisi` (tekst Obriši knjigu). Kod:

```csharp
protected void btnObrisi_Click(object sender, EventArgs e)
{
    string naslov = txtObrisi.Text.Trim();
    if (naslov == "")
    {
        lblBrisanje.ForeColor = Color.Red;
        lblBrisanje.Text = "Unesite naslov knjige.";
        return;
    }

    DataSet ds = UcitajKnjige();
    string filter = "naslov = '" + naslov.Replace("'", "''") + "'";
    DataRow[] nadjeni = ds.Tables[0].Select(filter);

    if (nadjeni.Length == 0)
    {
        lblBrisanje.ForeColor = Color.Red;
        lblBrisanje.Text = "Knjiga nije pronađena.";
    }
    else
    {
        foreach (DataRow red in nadjeni)
        {
            red.Delete();
        }
        ds.WriteXml(Putanja());
        lblBrisanje.ForeColor = Color.Green;
        lblBrisanje.Text = "Uspešno brisanje";
        txtObrisi.Text = "";
    }
    Prikazi(ds);
}
```

- `Select` prima uslov, sličan delu iza WHERE u SQL-u, i vraća niz redova `DataRow[]`. Tekst je pod jednostrukim navodnicima.
- `Select` podrazumevano ne razlikuje velika i mala slova: „na drini ćuprija“ nađe knjigu Na Drini ćuprija.
- `Delete` označi red kao obrisan; `WriteXml` posle toga upisuje samo redove koji su ostali.
- Ako dve knjige imaju isti naslov, obe se brišu.

### Pretraga po godini

Niz `DataRow[]` GridView ne ume da prikaže kao tabelu. Za prikaz filtriranih redova koristimo `DataView` – pogled na tabelu kroz filter. Uslov u `RowFilter` piše se isto kao u `Select`.

```csharp
protected void btnTrazi_Click(object sender, EventArgs e)
{
    DataSet ds = UcitajKnjige();
    DataView pogled = new DataView(ds.Tables[0]);
    pogled.RowFilter = "godina = '" + txtTrazi.Text.Trim().Replace("'", "''") + "'";

    GridView1.DataSource = pogled;
    GridView1.DataBind();
    lblPretraga.Text = "Pronađeno: " + pogled.Count;
}
```

Sve što DataSet pročita iz XML-a je tekst, zato je 2021 pod navodnicima. Za poređenje veće/manje tekst pretvori u broj, a za redosled koristi `Sort`:

```csharp
pogled.RowFilter = "Convert(godina, 'System.Int32') >= 1950";
pogled.Sort = "naslov ASC";
```

Kad pretraga ne nađe ništa, GridView nestane sa stranice. Dodaj mu `EmptyDataText="Nema knjiga."` i umesto tabele će se ispisati ta poruka.

### Zamke

| Zamka | Šta se desi | Rešenje |
| --- | --- | --- |
| Apostrof u tekstu, npr. Alice's Adventures in Wonderland | uslov u `Select` je neispravan, žuti ekran greške | `Replace("'", "''")` udvaja apostrof, kao u SQL-u |
| Obrisane su sve knjige | u fajlu ostane `<biblioteka />`, DataSet nema tabelu i `ds.Tables[0]` javi grešku Cannot find table 0 | u `UcitajKnjige` proveri `ds.Tables.Count == 0` i napravi tabelu (kod ispod) |
| Fajl ima ugnežđene elemente (adresa) | DataSet napravi više tabela i `Tables[0]` nije ona koju očekuješ | koristi ravan fajl ili tabelu po imenu: `ds.Tables["kontakt"]` |
| Greška u XML fajlu | `ReadXml` javi grešku i stranica pukne | proveri fajl u pregledaču; po želji `try`/`catch` kao kod baze |

Dopuna metode `UcitajKnjige` za prazan fajl:

```csharp
if (ds.Tables.Count == 0)
{
    ds.DataSetName = "biblioteka";
    DataTable tabela = ds.Tables.Add("knjiga");
    tabela.Columns.Add("naslov");
    tabela.Columns.Add("autor");
    tabela.Columns.Add("godina");
}
```

Sada uradi **Vežbe B3, B4 i B5**.

## Vežbe – blok 2

Svaka vežba je novi Web Site (ili nova stranica u istom projektu), a XML fajl je u App\_Data.

### B1 – Automobili u GridView-u

Napravi automobili.xml sa najmanje tri automobila. Svaki ima marku, model, godište i boju. Dugme Učitaj prikazuje ih u GridView-u.

```xml
<?xml version="1.0" encoding="utf-8"?>
<automobili>
  <automobil>
    <marka>Zastava</marka>
    <model>Yugo 45</model>
    <godiste>1988</godiste>
    <boja>crvena</boja>
  </automobil>
</automobili>
```

Dodatno: drugo dugme učitava knjige.xml u isti GridView. Kolone se menjaju same – zašto?

### B2 – Dodavanje filma

Napravi filmovi.xml u kome film ima naslov, godinu i žanr (bez ugnežđenih žanrova). Tri TextBox-a i dugme Dodaj film upisuju novi film u fajl. Naslov ne sme biti prazan, a godina mora biti ceo broj; o uspehu i grešci obavesti korisnika Label-om.

```xml
<filmovi>
  <film>
    <naslov>Ko to tamo peva</naslov>
    <godina>1980</godina>
    <zanr>komedija</zanr>
  </film>
</filmovi>
```

### B3 – Pretraga filmova po godini

TextBox i dugme Traži prikazuju u GridView-u samo filmove iz unete godine (`DataView` i `RowFilter`). Label ispisuje koliko je filmova pronađeno.

Dodatno: prikaži filmove snimljene posle unete godine, sortirane po naslovu.

### B4 – Brisanje filma

TextBox i dugme Obriši film brišu film po naslovu. Ako film ne postoji, ispiši crvenu poruku. Proveri šta se dešava kad obrišeš sve filmove i popravi to.

### B5 – Autori i njihove knjige

U App\_Data stavi dva fajla: autori.xml (id, ime, godina rođenja) i knjige.xml (naslov, autorId, godina). Na stranici su dva GridView-a jedan pored drugog: levo autori, desno knjige. Oba se popune kad se stranica otvori (`Page_Load`, `if (!IsPostBack)`).

```xml
<autori>
  <autor>
    <id>1</id>
    <ime>Ivo Andrić</ime>
    <rodjen>1892</rodjen>
  </autor>
</autori>
```

```xml
<knjige>
  <knjiga>
    <naslov>Na Drini ćuprija</naslov>
    <autorId>1</autorId>
    <godina>1945</godina>
  </knjiga>
</knjige>
```

Dodatno: levom GridView-u postavi `AutoGenerateSelectButton="true"` i `DataKeyNames="id"`. U metodi za obradu događaja `SelectedIndexChanged` desni GridView prikazuje samo knjige izabranog autora; id izabranog autora je `gvAutori.SelectedDataKey["id"]`.

## Podsetnik i pitanja za ponavljanje

### Kod na jednom mestu

| Zadatak | Kod |
| --- | --- |
| Putanja do fajla | `Server.MapPath("~/App_Data/knjige.xml")` |
| Učitaj fajl | `DataSet ds = new DataSet(); ds.ReadXml(putanja);` |
| Prikaži | `GridView1.DataSource = ds.Tables[0]; GridView1.DataBind();` |
| Novi red | `DataRow r = ds.Tables[0].NewRow(); r["naslov"] = "..."; ds.Tables[0].Rows.Add(r);` |
| Sačuvaj fajl | `ds.WriteXml(putanja);` |
| Nađi redove | `DataRow[] niz = ds.Tables[0].Select("naslov = 'Tvrđava'");` |
| Obriši red | `red.Delete();` pa `ds.WriteXml(putanja);` |
| Filtriraj prikaz | `DataView v = new DataView(ds.Tables[0]); v.RowFilter = "godina = '1970'";` |
| Proveri broj | `int.TryParse(txtGodina.Text, out godina)` |

### Pitanja

1. Šta znači „eXtensible“ u imenu XML?
2. Navedi tri razlike između XML-a i HTML-a.
3. Nabroj šest pravila dobro formiranog XML dokumenta.
4. Kako se u tekstu XML dokumenta zapisuje znak &?
5. Koja je razlika između dobro formiranog i validnog dokumenta?
6. Kada podatak pišeš kao atribut, a kada kao element?
7. Kad DataSet čita knjige.xml, šta postaje tabela, šta kolona, a šta red?
8. Čemu služi `Server.MapPath` i zašto XML fajl stoji u App\_Data?
9. Koja četiri koraka dodaju knjigu u XML fajl?
10. Zašto za brisanje koristimo `Select`, a za prikaz rezultata pretrage `DataView`?
11. Šta se desi kad obrišeš sve knjige i kako to rešavamo?
