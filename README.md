# PrimeLead - Dokumentacja API v1.1

## Reklamodawca

### Dostęp do API

Aby uzyskać dostęp do API PrimeLead, należy skontaktować się z obsługą techniczną. Reklamodawca uzyska unikalny identyfikator. Identyfikatora nie należy przekazywać osobom trzecim. Zabrania się także jego rozpowszechniania. 

### Generowanie UUID Akcji i raportowanie statusu

W wyniku kliknięć w materiały reklamowe opublikowane przez Wydawców, użytkownik jest jest kierowany na stronę Reklamodawcy. Podczas przekierowania użytkownika w systemie PrimeLead generowane jest **UUID Akcji**. Identyfikator jest przekazywany Reklamodawcy w formie parametru GET **utm_action** URL'a. Podsługując się identyfikatorem Reklamodawca aktualizuje status Akcji w systemie PrimeLead za pomocą Postback'ów lub dedykowanej metody API.

### Postback

Wersja DEV: **GET** `https://dev-ads.primelead.net/api/v1/postback/{status}/{action_uuid}`<br />
Wersja PROD: **GET** `https://ads.primelead.net/api/v1/postback/{status}/{action_uuid}`


| Parametr routingu  | Typ | Wartość | Opis |
| ------------- | ------------- | ------------- | ------------- |
| `{status}`  | string | wide_contact<br />narrow_contact<br />application<br />contract<br />contract_canceled | Raportowany status Akcji  |
| `{action_uuid}`  | string |  | UUID Akcji |


#### Parametry

Żądanie typu GET w zależności od akcji powinno zawierać parametry:

| Parametr url  | Typ | Wartość | Opis |
| ------------- | ------------- | ------------- | ------------- |
| `{customer_type}`  | NULL<br />string| NULL<br />returning<br />new | Rodzaj klienta (nowy/powracający)  |
| `{contract_net_value}`  | float | > 0 | Wartość netto umowy |
| `{contract_nominal_value}`  | float<br />NULL | > 0<br />NULL | Wartość brutto umowy |

Parametr `contract_net_value` jest wymagany w trakcie zmiany statusu Akcji na `contract`.

Przykład wywołania: `https://dev-ads.primelead.net/api/v1/postback/wide_contact/309c14b9-1ebb-4975-a608-7bf2ae517f2b?customer_type=new&contract_net_value=5000.00`<br />

### API

Wersja DEV: **POST** `https://dev-ads.primelead.net/api/v1/actions/{advertiserUUID}`<br />
Wersja PROD: **POST** `https://ads.primelead.net/api/v1/actions/{advertiserUUID}`<br />

| Parametr routingu  | Typ | Wartość | Opis |
| ------------- | ------------- | ------------- | ------------- |
| `{advertiserUUID}`  | string |  | UUID Reklamodawcy  |

#### Parametry

Żądanie typu POST powinno zawierać:

| Parametr url  | Typ | Wartość | Opis |
| ------------- | ------------- | ------------- | ------------- |
| `{actionUuid}`  | string || UUID Akcji  |
| `{customerType}`  | NULL<br />string | NULL<br />returning<br />new | Rodzaj klienta (nowy/powracający) |
| `{wideContact}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | Data zapisania Kontaktu |
| `{narrowContact}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | Data weryfikacji Kontaktu |
| `{application}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | Data złożenia Wniosku |
| `{contract}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | Data podpisania Umowy |
| `{contractNetValue}`  | NULL<br />float | > 0 | Wartość netto Umowy |
| `{contractNominalValue}`  | NULL<br />float | NULL<br />> 0 | Wartość brutto Umowy |
| `{contractCanceled}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | Data odstąpienia od Umowy |


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

