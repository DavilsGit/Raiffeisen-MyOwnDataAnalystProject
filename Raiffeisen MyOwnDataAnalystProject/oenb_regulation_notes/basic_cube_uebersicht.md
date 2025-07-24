mindmap
  root((Basic Cube: Datenmodell für Meldewesen))
    Einleitung
      Beschreibung des Ziels
      Bezug zum ERD (Entity-Relationship Diagramm)
      Hinweis auf Detailbeschreibungen
      Status und Weiterentwicklung des Datenmodells
    Konzept des Basic Cubes
      Definition
        Redundanzfreie Darstellung (dritte Normalform)
        Abbildung aller melderelevanten Inhalte
        "Passiv Data" (Ad-hoc Auskünfte)
        "Enrichment" (Ableitung von Inhalten)
      Zweck
        Grundlage für Meldungen an die OeNB
        Vermeidung von Doppelzählungen
      Wichtige Hinweise
        Anpassung bei unterschiedlicher Darstellung von Geschäftsfällen
        Kundennähe und Abbildung an Abwicklungssystemen
    Mandant & Meldeobjekt
      Mandant
        Definition (meldetechnisch)
        Identifikation: (AI_Mandant, AI_Stichtag_Datum)
      Meldeobjekt
        Definition (Konzernsicht)
        Identifikation: AI_Kons_ID
      Abgrenzung
        Mandant vs. Rechenzentrums-Mandant
        Beziehung zu Hauptanstalt/Zweigstelle
    Schnappschuss zum Berichtstermin
      AI_Stichtag_Datum
      Abbildung von Geschäftsfällen und Sachkonten
    Überblick über Inhalte und deren Modellierung
      Zentrale Entitäten
        GF_Geschaeftsfall
        SK_Sachkonto
      Weitere Themen
        Darstellung Zusammenfassung/Konsolidierung von Einheiten
        Darstellung von Werten
        Reporting Packages
        Darstellung von Sicherheiten

    ---

    Geschäftsfall (GF_Geschaeftsfall)
      Definition
        Vereinbarung zwischen Partnern (finanzielle Rechtsverhältnisse)
      Identifikation
        (AI_Geschaeftsfall_ID, AI_Mandant, AI_Stichtag_Datum)
      Zentrale Rolle
        Basis für melderelevante Informationen
      Beziehungen zu anderen Entitäten
        EM_Einheit_MS (über KR_Kundenrollen)
        GFW_Geschaeftsfall_Wert
        GE_Geschaeftsfall_Exposure
        SZ_Sicherheiten_Zerlegung
        ST_Sicherheiten_Stammdaten
        GB_Geschaeftsfall_Sachkonto_Sicherheiten_Beziehung
        WM_Wertpapier_MS
        VT_Verweildauer
        RZ_Restlaufzeitentabelle
        EE_Ereignis
      Attribute
        Originäre Attribute (z.B. GF17_Geschaeftsbeginn_Datum)
        Abgeleitete Attribute (z.B. GFA83_Ursprungslaufzeit_Code, GFA84_vertragliche_Restlaufzeit_Code)
        Bilanzbezogene Attribute (z.B. GF132_Bilanzposition_local_GAAP_Code, GKA12_Bilanzposition_FinRep_NGAAP_Code)
      Melderelevanz
        Bilanzwirksame Geschäftsfälle (in/unter der Bilanz)
        Geschlossene Geschäftsfälle mit Veränderungen
        Besondere Konstellationen (Leadmanagement, verkaufte/servicierte Kredite)
      Spezialfälle
        Rahmen
        Pool-Zuordnung (AI_Geschaeftsfall_Sicherheiten_Sachkonten_Pool_ID, AI_Mandant2)
        Geschäfte zwischen Hauptanstalt und Zweigstelle
        Ausländische Töchter (Reporting_Package)

    ---

    Sachkonto (SK_Sachkonto)
      Definition
        Melderelevante Informationen, die keine Geschäftsfälle sind
        Strukturierter als Kennzahlen
        Kein direktes Counterpart
      Pool-Zuordnung
        AI_Geschaeftsfall_Sicherheiten_Sachkonten_Pool_ID
        AI_Mandant2
      Klassifikation
        Sachkontokategorie_SL
      Beziehung zu EM_Einheit_MS
        Über KR_Kundenrollen (Kunstgriff für Gliederung z.B. nach ESVG - Sektor)

    ---

    Einheit (EM_Einheit_MS)
      Definition
        Physische oder juristische Person (oder Zusammenfassung)
        Beteiligung an Geschäftsfall oder Sachkonto in relevanter Rolle
      Identifikation
        (AI_Einheitennummer_ID, AI_Stichtag_Datum, AI_Mandant)
      Zweck
        Verwaltung von Einheiten und deren Eigenschaften
        Grundlage für Gliederungen (Kundeneigenschaften)
      Attribute
        Identifikation (EM01_Name_MS, EM12_Rechtsform_MS_Code etc.)
        Geografische Zuordnung (EM02_Sitzland_MS_Code, EM17_Steuerland_Code etc.)
        Klassifikation (EM04_Sektor_ESVG_MS_Code, EM06_OENACE_MS_Code etc.)
        Risikoaspekte (EM19_Rueckzahlung_unwahrscheinlich_MS_Kennzeichen, EM20_90_Tage_ueberfaellig_MS_Kennzeichen etc.)
      Beziehungen
        KR_Kundenrollen (1:n Beziehung)
        RS_Ratingsystem_intern
        BK_Bonitaetsklasse_intern
        EZ_Einheiten_Zusammenfassung_MS
      Spezialfälle
        Ausländische Zweigstellen als eigene Datensätze
        Dummy-Einheit (z.B. bei anonymen Sparbüchern, unbekannten Investoren)
      OeNB - Stammdaten (SSD)
        Standardisierte Stammdaten-Meldung
      Rating
        EM65_Rating_standardisiert_Code (ECAI-Ratings)
        AI_Ratingsystem, AI_Bonitaetsklasse (institutsinterne Ratings)

    ---

    Kundenrolle (KR_Kundenrollen)
      Definition
        Rolle, die ein Kunde bezüglich eines Geschäftsfalles, Sachkontos oder einer Sicherheit spielt
      Codeliste
        Rolle_SL
      Identifikation
        (AI_Mandant, AI_Stichtag_Datum, AI_Geschaeftsfall_ID/AI_Sachkonto_ID/AI_Sicherheiten_ID)
        AI_Rolle_Code
      Beziehung zu GF_Geschaeftsfall (c:n Beziehung)
      Beziehung zu EM_Einheit_MS (1:n Beziehung)
      Beispiele für Rollen
        Inhaber (IH)
        Treugeber
        Begünstigter
        Co-Inhaber (CI)
        Sicherungsgeber (SG)
        Der Sicherheit zugeordnete Einheit (SE)
      Wichtige Hinweise
        Nicht jede Kundenrolle einem Geschäftsfall zugeordnet (z.B. Sicherheit)
        Dummy-Einheit bei unbekanntem Kunden (z.B. Emittent bei Wertpapieren)

    ---

    Pensionsgeschäfte
      Abhängig von Art des Geschäfts und Mandantensicht
      "echte Pensionsnahmen" (ähnlich besichertem Geschäftsfall)
      "echte Pensionsgaben" (zwei verbundene Geschäftsfälle)
      Referenz: Kapitel "Darstellung von Pensionsgeschäften"

    ---

    Wertpapierleihe-Geschäfte
      Abhängig von Art und Mandantensicht
      Referenz: Kapitel "Darstellung von Leihegeschäften"

    ---

    Geschäftsfall_Wert (GFW_Geschaeftsfall_Wert)
      Definition
        Abbildung von Werten zu einem Geschäftsfall (z.B. Buchwert, Marktwert, Einzelwertberichtigung)
      Charakterisierung der Werte
        AI_Wertart_Code (Referenz: Wertart_CL)
        AI_Wertmesseinheit_Code (Referenz: Wertmesseinheit_SL)
        WT03_UGB_IFRS_Code

    ---

    Wertpapierstammdaten (WM_Wertpapier_MS)
      Anwendung
        Geschäftsfallkategorien "Wertpapiere (H)", "Investmentfonds (I)", "Verbriefung (J)", "Lead Management (S)"
      Identifier
        ISIN oder interne Wertpapierkennnummer
      Beziehung zu GF_Geschaeftsfall (c:cm Beziehung)

    ---

    Verweildauertabelle (VT_Verweildauer)
      Zweck
        Angabe, wie lange Einlagen vom Kunden beim KI belassen werden (Prozentsätze)
      Anwendung
        "täglich fällige" Einlagen, kurze Bindungsfristen, Spareinlagen (GF00_Geschaeftsfallkategorie_Code = „Einlage“ && GFA83_Ursprungslaufzeit_Code = "keine Frist")
      Identifier
        (AI_Verweildauer_ID, AI_Mandant, AI_Stichtag_Datum)
      Beziehung zu GF_Geschaeftsfall (n:cm Beziehung)
      Ermittlung
        Statistische Methoden (z.B. Jahresumsätze)
      Attribute
        VT01_Prozentsatz
        Restlaufzeiten (z.B. "bis keine Frist", "bis 1 Monat")
      AI_Verweildauertyp

    ---

    Restlaufzeitentabelle (RZ_Restlaufzeitentabelle)
      Zweck
        Darstellung zukünftiger Zahlungsströme je Geschäftsfall (Tilgungsplan)
      Identifikation
        (AI_Geschaeftsfall_ID, AI_Stichtag_Datum, AI_Mandant, AI_Restlaufzeitentabelle_ID)
      Anwendung (relevante Geschäftsfallkategorien)
        Einmalkredit, Kreditlinie, Wechselkredit, Kreditkartenkredit, Revolvierender Kredit, Überziehungskredit, Barvorlagen, Operating Leasing, Finance Leasing, Einlagen, Wertpapiere (Schuldverschreibung, CLN), Verbriefung
      Konventionen
        Keine Restlaufzeitentabelle für endfällige Fälle (Verwendung GF52_Geschaeftsende_Datum, GF19_Kapitalende_Datum, GF82_Kuendigungsfrist_Code)
        Restlaufzeitentabelle für "bilanzielle Kredite"
        Berücksichtigung von Einzelwertberichtigungen
      Beziehung zu GF_Geschaeftsfall (n:cm Beziehung)
      Zugehörige Werte
        RZW_Restlaufzeitentabelle_Wert
        XUB_Kapital_Restlaufzeit (Wertart "Kapital-Restlaufzeit-Wert (RT)")
      Attribute
        RZA02_Tilgungsplan_Code
      Hinweise
        Auch für überfällige Instrumente
        Lineare Approximation bei großen Datenmengen
        Konsistenz mit Buchwerten und Einzelwertberichtigungen

    ---

    Ereignis (EE_Ereignis)
      Zweck
        Zusatzinformationen zu einem GF_Geschaeftsfall innerhalb einer Berichtsperiode
        Abbildung von Veränderungen
      Beispiele
        Neuverhandlung Vertragsinhalte, Währungsänderung/Konvertierung
      Beziehung zu GF_Geschaeftsfall (cn:1 Beziehung)

    ---

    Geschäftsfall_Sachkonto_Sicherheiten_Beziehung (GB_Geschaeftsfall_Sachkonto_Sicherheiten_Beziehung)
      Zweck
        Abbildung von Beziehungen zwischen Geschäftsfällen, Sachkonten, Sicherheiten (fachliche Zusammengehörigkeiten)
      Beispiele
        Pensionsgeschäfte
      Attribute
        GB01_Beziehungsart_Code ("Zerlegung", "Underlying")
        AI_Geschaeftsfall_ID (übergeordneter Geschäftsfall)
        AI_Geschaeftsfall_ID2 (untergeordnete Geschäftsfälle)
      Beziehung zu GF_Geschaeftsfall (cn:m Beziehung)

    ---

    Sicherheiten und Exposure (für "Risikoinformationen")
      Entitäten
        GE_Geschaeftsfall_Exposure
        ST_Sicherheiten_Stammdaten
        SZ_Sicherheiten_Zerlegung
      Zweck
        Aufsichtsrechtliche Zwecke (COREP, FINREP)
        Abbildung von Eigenmittelanforderungen

    ---

    Geschäftsfall_Exposure (GE_Geschaeftsfall_Exposure)
      Zweck
        "Nach-Rechenkern"-Informationen (Eigenmittelanforderung für Kredit-/Kontrahentenausfallsrisiko gem. CRR)
        Aufteilung in Risikopositionsklassen (GE07_Risikopositionsklasse_Corep_Code)
        On-balance/Off-balance (Forderungskategorie)
      Identifikation
        (AI_Stichtag_Datum, AI_Mandant / AI_Kons_ID)
      Beziehung zu GF_Geschaeftsfall (cn:1 Beziehung)
      Zugehörige Werte
        GEW_Geschaeftsfall_Exposure_Wert
      Ermittlungsgrundlage
        ST_Sicherheiten_Stammdaten und SZ_Sicherheiten_Zerlegung

    ---

    Sicherheiten_Zerlegung (SZ_Sicherheiten_Zerlegung)
      Zweck
        Zuordnung von Sicherheiten zu Geschäftsfall-Exposures
      Identifikation
        AI_Exposure_ID
      Beziehung zu GE_Geschaeftsfall_Exposure
      Beziehung zu ST_Sicherheiten_Stammdaten (1:cn Beziehung)
      Zugehörige Werte
        SZW_Sicherheiten_Zerlegungs_Wert
      Hinweise
        Abbildung von Immobiliensicherheiten (auch Wert = 0)
        Modellierung der verwendeten Sicherheit und deren Wert

    ---

    Sicherheiten_Stammdaten (ST_Sicherheiten_Stammdaten)
      Zweck
        Abbildung verschiedener Sicherheiten
      Identifikation
        (AI_Sicherheiten_ID, AI_Mandant, AI_Stichtag_Datum, AI_Kons_ID)
      Klassifikation
        ST03_Sicherheitenkategorie_Code
      Beziehung zu KR_Kundenrollen
        Mittels AI_Rolle_Code = "Der Sicherheit zugeordnete Einheit (SE)"
      Pool-Zuordnung
        AI_Geschaeftsfall_Sicherheiten_Sachkonten_Pool_ID
        AI_Mandant2
      Beziehung zu SZ_Sicherheiten_Zerlegung (1:cn Beziehung)
      Zugehörige Werte
        STW_Sicherheiten_Stammdaten_Wert
      Hinweise
        Stellt die Immobilie dar, nicht das Pfandrecht

    ---

    Einheiten_Zusammenfassung (EZ_Einheiten_Zusammenfassung_MS)
      Zweck
        Zusammenfassen von Einheiten (z.B. "Gruppen verbundener Kunden", "Beteiligungskreis", "Hauptanstalt-Zweiganstalt Beziehung")

    ---

    Werte im Datenmodell (Überblick)
      Spezielle Wert-Entitäten für jede Wert-relevante Entität
        GFW_Geschaeftsfall_Wert
        GEW_Geschaeftsfall_Exposure_Wert
        SKW_Sachkonten_Wert
        STW_Sicherheiten_Stammdaten_Wert
        SZW_Sicherheiten_Zerlegungs_Wert
      Charakterisierung der Werte
        AI_Wertart_Code (Wertart_CL)
        AI_Wertmesseinheit_Code (Wertmesseinheit_SL)
        WT03_UGB_IFRS_Code (UGB und IFRS Differenzierung)