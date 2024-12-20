# ETL proces datasetu Northwind

Tento repozitár obsahuje implementáciu ETL procesu v Snowflake pre analýzu dát z datasetu Northwind. Projekt sa zameriava na analýzu predajných trendov, preferencií zákazníkov a optimalizáciu logistických procesov na základe údajov o kategóriách produktov, zákazníkoch, zamestnancoch, objednávkach a dodávkach. Výsledný dátový model umožňuje multidimenzionálnu analýzu a vizualizáciu kľúčových metrík.  

## 1. Úvod a popis zdrojových dát   

Cieľom semestrálneho projektu je analyzovať dáta týkajúce sa kategórií produktov, zákazníkov, zamestnancov, objednávok a dodávok. Táto analýza umožňuje identifikovať trendy v predaji, preferenciách zákazníkov a optimalizovať procesy v rámci spoločnosti.

Zdrojové dáta pochádzajú z datasetu Northwind dostupného [tu](https://edu.ukf.sk/mod/folder/view.php?id=252869). Dataset obsahuje nasledujúce hlavné tabuľky:  

- categories
- products
- suppliers
- orderdetails
- shippers
- orders
- employees
- customers

Categories — obsahuje informácie o kategóriách produktov:
- CategoryID — unikátny identifikátor kategórie.
- CategoryName — názov kategórie.
- Description — popis kategórie.

Customers — údaje o zákazníkoch:
- CustomerID — unikátny identifikátor zákazníka.
- CustomerName — názov organizácie alebo meno zákazníka.
- ContactName — kontaktná osoba.
- Address — adresa zákazníka.
- City — mesto zákazníka.
- PostalCode — poštové smerovacie číslo.
- Country — krajina zákazníka.

Employees — informácie o zamestnancoch:
- EmployeeID — unikátny identifikátor zamestnanca.
- LastName — priezvisko zamestnanca.
- FirstName — meno zamestnanca.
- BirthDate — dátum narodenia zamestnanca.
- Photo — cesta k obrázku zamestnanca.
- Notes — dodatočné poznámky o zamestnancovi.

Shippers — údaje o spoločnostiach, ktoré zabezpečujú prepravu:
- ShipperID — unikátny identifikátor dopravcu.
- ShipperName — názov spoločnosti.
- Phone — kontaktné telefónne číslo.

Suppliers — informácie o dodávateľoch:
- SupplierID — unikátny identifikátor dodávateľa.
- SupplierName — názov spoločnosti dodávateľa.
- ContactName — kontaktná osoba dodávateľa.
- Address — adresa dodávateľa.
- City — mesto dodávateľa.
- PostalCode — poštové smerovacie číslo.
- Country — krajina dodávateľa.
- Phone — kontaktné telefónne číslo.

Products — obsahuje informácie o produktoch:
- ProductID — unikátny identifikátor produktu.
- ProductName — názov produktu.
- SupplierID — identifikátor dodávateľa (vzťah s tabuľkou Suppliers).
- CategoryID — identifikátor kategórie produktu (vzťah s tabuľkou Categories).
- Unit — jednotka merania produktu.
- Price — cena produktu.

Orders — údaje o objednávkach:
- OrderID — unikátny identifikátor objednávky.
- CustomerID — identifikátor zákazníka, ktorý zadal objednávku (vzťah s tabuľkou Customers).
- EmployeeID — identifikátor zamestnanca, ktorý spracoval objednávku (vzťah s tabuľkou Employees).
- OrderDate — dátum zadania objednávky.
- ShipperID — identifikátor dopravcu, ktorý doručil objednávku (vzťah s tabuľkou Shippers).

OrderDetails — podrobnosti objednávok:
- OrderDetailID — unikátny identifikátor záznamu.
- OrderID — identifikátor objednávky (vzťah s tabuľkou Orders).
- ProductID — identifikátor produktu (vzťah s tabuľkou Products).
- Quantity — množstvo objednaného produktu.

Účelom ETL procesu bolo tieto dáta pripraviť, transformovať a sprístupniť pre viacdimenzionálnu analýzu.

### 1.1 Dátová architektúra
ERD diagram
Surové dáta sú usporiadané v relačnom modeli, ktorý je znázornený na entitno-relačnom diagrame (ERD):

![Entitno-relačná schéma Northwind_ERD](https://github.com/Wilkwarin/Walmart_Sales_Forecast-ETL/blob/main/Northwind_ERD.png)

*Obrázok 1 Entitno-relačná schéma Northwind_ERD*

---

# 2 Dimenzionálny model
