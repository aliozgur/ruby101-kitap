# Methods (Fonksiyonlar)

Programlama parçacıklarının ve/veya ifadelerin biraraya toplandığı şeydir metod. Aslında lise matematiğinden hepimiz aşinayız.

Bildiğiniz matematik fonksiyonu. Bundan böyle fonksiyon yerine **metod** kullanacağım. Çünkü Ruby demek neredeyse **Obje** (_Nesne_) ve **Metod** (_Method_) demek.

Önceki konularda gördüğümüz operatörlerin neredeyse hepsi bir metod! Hemen **Method Definition**'a yani nasıl metod tanımlandığına bir göz atalım.

```ruby
def merhaba
  "Merhaba"
end

merhaba        # => "Merhaba"
```

`def` ve `end` anahtar kelimeleri arasına metodun adı geldi. Önce metodu tanımladık, sonra çağırdık.

Ruby'de herşey mutlaka **geriye birşey döner**. Ne demek bu? Prensip olarak metodlar zinciri olarak çalıştığı için, metod denen şey de aslında bir fonksiyon ve fonksiyon denen şey de bir dizi işlemin yapılıp geriye sonucun dönüldüğü bir taşıyıcı aslında.

Hemen `irb` ye geçelim:

    irb(main):001:0> puts "Merhaba"
    Merhaba
    => nil
    irb(main):002:0>


`puts "Merhaba"` önce işini yaptı ve çıktı olarak **Merhaba** verdi. sonra `=> nil` dikkatinizi çekti mi?

Çünkü `puts` metodu işini yaptı bitirdi ve geriye `nil` döndü! Peki daha önceki programlama tecrübelerimize dayanak, **geriye döndü** işini hangi komut yapmış olabilir?

Pek çok dilde fonksiyondan birşey geri dönmek için **return** kelimesi kullanılır. Ruby'de de kullanılır ama zorunlu değildir. Yukarıdaki `def merhaba` örneğinde `return` kullanmamamıza rağmen geriye **Merhaba** dönebildi.

İşte bu Ruby'nin özelliği. Kodu okurken bunu bilmezsek kafamız süper karışabilir.

`def` ile tanımlanan metodu, `undef` ile yokedebilirsiniz.

```ruby
def merhaba
  "Merhaba"
end

merhaba # => "Merhaba"

undef merhaba

merhaba # =>
# ~> -:9:in `<main>': undefined local variable or method `merhaba' for main:Object (NameError)
```

Gördüğünüz gibi `undefined local variable or method .. Object (NameError)` oldu.

Metodlar argüman alabilir. Yani fonksiyona, doğal olarak, parametre geçebilirsiniz.

```ruby
def merhaba(isim)
  "Merhaba #{isim}"
end

merhaba("vigo") # => "Merhaba vigo"
```
aynı örneği aşağıdaki gibi de yazabiliriz:

```ruby
def merhaba isim
  "Merhaba #{isim}"
end

merhaba "vigo" # => "Merhaba vigo"
```

Metodu tanımlarken ve çağırırken **parantez** kullanmadık! Bu durumda alışmanız gereken önemli konulardan. Şahsen ben, daha önce hiçbir programlama dilinde böyle birşey görmedim!

Bazı durumlara, argüman alan metod çağırırken, argümanın tipine göre, eğer parantez kullanmadan çağırma yaparsanız **warning** alabilirsiniz!

## Metod Yazma Kuralları (_Method Conventions_)

Ruby, pek çok konuda rahat gibi görünse bile bazı kuralları var tabi. Özellikle metodların son karakteri ile ilgili. Eğer bir metodun son karakteri `?` ise bu o metodun `true` ya da `false` yani **Boolean** bir değer döneceğini ifade eder.

```ruby
a = "ali"
b = "ali"

a.eql? b  # => true
a.eql?(b) # => true
```

`.eql?` metodu eşitliği kontrol eder ve mutlaka sonuç **Boolean** döner.

Eğer metodun son karakteri `!` (_Ünlem_) ise; bu, o metodun tehlikeli bir iş yaptığını anlatır. Yani ilgili nesnenin kopyalanmadan direk üzerinde değişiklik yapacağı anlamına gelir.

```ruby
a = "deneme"

a.upcase   # => "DENEME"
a          # => "deneme"

a.upcase!  # => "DENEME"
a          # => "DENEME"
```

`a` değeri **deneme**. `.upcase` ile orijinal değeri değiştirmeden **uppercase** (_Büyük harf_) yaptık. Değeri kontrol ettiğimizde halen küçük harf olduğunu gördük. `.upcase!` kullandığımız anda değişkenin orijinal değerini de bozduk.

Eğer bir metod `=` ile bitiyorsa, bu, o metodun bir **setter** metodu olduğu anlamına gelir ve **Class** ile ilgili bir konudur.

```ruby
class User
  def email=(email)
    @email = email
  end
end

u = User.new
u # => #<User:0x007ff7229ed880>

u.email = "vigo@xyz.com"
u # => #<User:0x007ff7229ed880 @email="vigo@xyz.com">
```

## Varsayılan Argümanlar (_Default Arguments_)

Metod argümanlarına varsayılan değerler atayabilirsiniz. Bu, eğer geçilmesi beklenen argüman gelmemişse otomatik olarak değer atamsı yapmayı sağlar.

```ruby
def merhaba(isim="insalık!")
  "Merhaba #{isim}"
end

merhaba          # => "Merhaba insalık!"
merhaba("vigo")  # => "Merhaba vigo"
```
Parametre geçmenden çağırdığımızda, tanımladığımız varsayılan (_default_) değer atandı.

## Değişken Argümanları (_Variable Arguments_)

Bazı durumlarda metoda dinamik olarak parametre geçmek gerekebilir. Bu durumda argümanın başına `*` işareti gelir. Bu sayede o argüman artık bir dizi (_Array_) haline gelir.

```ruby
def merhaba(*isimler)
  "Merhaba #{isimler.join(" ve ")}"
end

merhaba("vigo")                        # => "Merhaba vigo"
merhaba("vigo", "yeşim", "ezel")       # => "Merhaba vigo ve yeşim ve ezel"

merhaba "dünya", "uzay", "evren", "ay" # => "Merhaba dünya ve uzay ve evren ve ay"
```

Keza, şu şekilde de kullanılabilir:

```ruby
def custom_numbers(first, second, *others)
  puts "ilk sayı: #{first}"
  puts "ikinci sayı : #{second}"
  puts "diğer sayılar : #{others.join(",")}"
end

custom_numbers 1,2,50,100 # => nil
# >> ilk sayı: 1
# >> ikinci sayı : 2
# >> diğer sayılar : 50,100
```

## Metoda Takma İsim Vermek (_Aliasing_)

Varolan bir metodu, başka bir isimle çağırmak. Bu aslında **Class** konusuyla çok ilintili ama kısaca değinmek istiyorum.

```ruby
def merhaba(isim)
  "Merhaba! #{isim}"
end

alias naber merhaba

merhaba "Uğur" # => "Merhaba! Uğur"
naber "Uğur"   # => "Merhaba! Uğur"
```

Formül şu: `alias` `TAKMA AD` `ORİJİNAL` yani `alias naber merhaba` derken, `merhaba` metoduna takma ad olarak `naber`i tanımladık!


### Unutma!

* `return` kullanmadan metoddan geri dönüş yapılabilir
* Parantez kullanmadan metod tanımlanabilir
* Parantez kullanmadan metod çağırılıp parametre geçilebilir.
* `?` ile biten metod mutlaka `true` ya da `false` döner.
* `!` ile biten orijinal değeri mutlaka değiştirir.
* `=` ile biten **setter**'dır.
