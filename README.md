# PrimeLead - Dokumentacja API v1

## Dostęp do API

Aby uzyskać dostęp do API PrimeLead, należy skontaktować się z obsługą techniczną. Reklamodawca uzyska unikalny identyfikator. Identyfikatora nie należy przekazywać osobom trzecim. Zabrania się także jego rozpowszechniania. 

## Sposoby aktualizacji akcji

W wyniku kliknięć w materiały reklamowe opublikowane przez Wydawców, użytkownik jest jest kierowany na stronę Reklamodawcy. Podczas przekierowania użytkownika w systemie PrimeLead generowane jest tzw. ID akcji. Identyfikator jest przekazywany Reklamodawcy w formie parametru GET **utm_action** URL'a. Podsługując się identyfikatorem Reklamodawca aktualizuje status Akcji w systemie PrimeLead za pomocą Postback'ów lub dedykowanej metody API.

## Postback

**GET** `https://dev-ads.primelead.net/api/v1/postback/{status}/{action_uuid}`

| Parametr routingu  | Typ | Wartość | Opis |
| ------------- | ------------- | ------------- | ------------- |
| `{status}`  | string | wide_contact<br />narrow_contact<br />application<br />contract<br />contract_canceled | nowy status akcji  |
| `{action_uuid}`  | string |  | UUID akcji |

### Parametry

Żądanie typu GET w zależności od akcji powinno zawierać parametry:

| Parametr url  | Typ | Wartość | Opis |
| ------------- | ------------- | ------------- | ------------- |
| `{customer_type}`  | NULL<br />string| NULL<br />returning<br />new | typ klienta  |
| `{contract_net_value}`  | float | > 0 | wartość kontraktu netto |
| `{contract_nominal_value}`  | float<br />NULL | > 0<br />NULL | wartość kontraktu brutto |

Parametr `contract_net_value` musi zostać przekazany przed lub w trakcie zmiany statusu akcji na `contract`.

## API

**POST** `https://dev-ads.primelead.net/api/v1/actions/{advertiserUUID}`

| Parametr routingu  | Typ | Wartość | Opis |
| ------------- | ------------- | ------------- | ------------- |
| `{advertiserUUID}`  | string |  | unikalny identyfikator reklamodawcy  |

### Parametry

Żądanie typu POST powinno zawierać:

| Parametr url  | Typ | Wartość | Opis |
| ------------- | ------------- | ------------- | ------------- |
| `{actionUuid}`  | string || UUID akcji  |
| `{customerType}`  | NULL<br />string | NULL<br />returning<br />new | typ klienta |
| `{wideContact}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | data pierwszego kontaktu |
| `{narrowContact}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | data weryfikacji kontaktu |
| `{application}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | data złożenia wniosku |
| `{contract}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | data podpisania kontraktu |
| `{contractNetValue}`  | NULL<br />float | > 0 | wartość kontraktu netto |
| `{contractNominalValue}`  | NULL<br />float | NULL<br />> 0 | wartość kontraktu brutto |
| `{contractCanceled}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | data zamknięcia kontraktu |

### Rejestrowanie akcji metodą POST

Istnieje możliwość tworzenia akcji poprzez API Prime Lead. Aby zarejestrować akcję należy zmodyfikować powyższe body metody POST:

| Parametr url  | Typ | Wartość | Opis |
| ------------- | ------------- | ------------- | ------------- |
| `{campaignUuid}`  | string || UUID kampanii  |
| `{actionCreatedAt}`  | string | data, format: Y-m-d H:i:s | data utworzenia akcji |
| `{publisherUuid}`  | string |  | UUID wydawcy |
| `{customerType}`  | NULL<br />string | NULL<br />returning<br />new | typ klienta |
| `{wideContact}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | data pierwszego kontaktu |
| `{narrowContact}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | data weryfikacji kontaktu |
| `{application}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | data złożenia wniosku |
| `{contract}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | data podpisania kontraktu |
| `{contractNetValue}`  | NULL<br />float | > 0 | wartość kontraktu netto |
| `{contractNominalValue}`  | NULL<br />float | NULL<br />> 0 | wartość kontraktu brutto |
| `{contractCanceled}`  | NULL<br />string | NULL<br />data, format: Y-m-d H:i:s | data zamknięcia kontraktu |

## Schemat odpowiedzi HTTP

Odpowiedź API Prime Lead przekazywana jest w postaci JSON oraz odpowiednim do danego przypadku kodem odpowiedzi HTTP. Struktura JSON zawiera wartości: 

"<em>success</em>" - boolean, określa czy dana akcja została wykonana pomyślnie;

"<em>message</em>" - string, słowny opis kodu odpowiedzi HTTP;

"<em>errors</em>" - array, zawiera listę napotkanych błędów walidacji.

## Kody odpowiedzi HTTP

Możliwe odpowiedzi API Prime Lead:

| kod  | <em>success</em> | <em>message</em> | opis/znaczenie |
| ------------- | ------------- | ------------- | ------------- |
| 200 | `true` | | Status akcji został pomyślnie zaktualizowany |
| 403 | `false` | Permission denied | Metoda niedostępna, brak dostępu |
| 422 | `false` | Validation error | Błąd walidacji |
| 409 | `false` | Conflict | Nieoczekiwany błąd |

W przypadku wystąpienia błędu 422 zostanie zwrócona także tablica "<em>errors</em>" z polami w których wystąpiły niepoprawne dane.


