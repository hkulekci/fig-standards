PHP-FIG tarafından yayımlanan kodu adlandırma kuralları
-------------------------------------------------------

1. Arayüzler(Interfaces) `Interface` sonekini almalıdırlar. Örn: `Psr\Foo\BarInterface`.
2. Soyut(Abstract) sınıflar `Abstract` önekini almalıdırlar. Örn: `Psr\Foo\AbstractBar`.
3. Trait'ler `Trait` son ekini almalıdırlar. Örn: `Psr\Foo\BarTrait`
4. PSR-0, 1 ve 2 standartlarını takip etmek zorunludur. 
5. vendor namespace `Psr` olmalıdır.
6. PSR ile ilişkide kodu kapsayan package/ikinici-seviye bir namespace olmalıdır. 
7. Composer paketleri `psr/<package>` şeklinde adlandırılmalıdır. Örn: `psr/log`. Eğer
   bir uygulama(implementation) gerektiriyorsa, `psr/<package>-implementation` şeklinde
   adlandırılmalıdır ve `1.0.0` şeklinde özel bir versiyon bilgisi gerektirmektedir.
   _Implementors of that PSR can then provide
   `"psr/<package>-implementation": "1.0.0"` in their package to satisfy that
   requirement. Specification changes via further PSRs should only lead to a new
   tag of the `psr/<package>` package, and an equal version bump of the
   implementation being required._