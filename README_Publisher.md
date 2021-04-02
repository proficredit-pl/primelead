# PrimeLead - Dokumentacja API v1.1

## Wydawca

### Dostęp do API

Aby uzyskać dostęp do API PrimeLead umożliwiającego Wydawcy wysyłkę leadów do Reklamodawcy, należy skontaktować się z obsługą techniczną. W zgłoszeniu należy podać numer IP hosta, z którego będą następowały wywołania API. Przy wywoływaniu metody API Wydawca posługuje się parametrem **publisherUUID**, możliwym do pobrania z profilu Wydawcy w systemie PrimeLead.

### Wysyłanie Kontaktów do Reklamodawcy

Wersja DEV: **POST** `https://dev-ads.primelead.net/api/v1/contacts/{publisherUUID}`<br />
Wersja PROD: **POST** `https://ads.primelead.net/api/v1/contacts/{publisherUUID}`

Aby zarejestrować Akcję w systemie PrimeLead i zainicjować wysyłkę leada do Reklamodawcy należy wywołać powyższą metodę, przekazując w BODY dane, które powinny zostać przekazane do Reklamodawcy.

## Schemat odpowiedzi HTTP

Odpowiedź API PrimeLead przekazywana jest w postaci JSON oraz odpowiednim do danego przypadku kodem odpowiedzi HTTP. Struktura JSON zawiera wartości: 

"<em>success</em>" - boolean, określa czy dana akcja została wykonana pomyślnie;

"<em>message</em>" - string, słowny opis kodu odpowiedzi HTTP;

"<em>errors</em>" - array, zawiera listę napotkanych błędów walidacji.

## Kody odpowiedzi HTTP

Możliwe odpowiedzi API PrimeLead:

| kod  | <em>success</em> | <em>message</em> | opis/znaczenie |
| ------------- | ------------- | ------------- | ------------- |
| 200 | `true` | | Status akcji został pomyślnie zaktualizowany |
| 403 | `false` | Permission denied | Metoda niedostępna, brak dostępu |
| 422 | `false` | Validation error | Błąd walidacji |
| 409 | `false` | Conflict | Nieoczekiwany błąd |

W przypadku wystąpienia błędu 422 zostanie zwrócona także tablica "<em>errors</em>" z polami w których wystąpiły niepoprawne dane.
