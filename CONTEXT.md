# RA-APPS Imobile Tracker

A git-scraping archive that preserves the official RA-APPS lists of state-owned
properties. Romania's RA-APPS publishes these lists twice a year and removes each
edition after six months; this tracker keeps the history that would otherwise be lost.

## Language

**RA-APPS**:
Regia Autonomă „Administrația Patrimoniului Protocolului de Stat" — the Romanian
state agency that administers state-owned real estate. The dataset's issuer.
_Avoid_: APPS (ambiguous with unrelated cadastral data on data.gov.ro)

**Imobil**:
A single state-owned property (building and/or land) administered by RA-APPS.
One row in a list.
_Avoid_: Asset, building (an imobil may be land only)

**List**:
One official PDF published by RA-APPS. There are exactly three per edition:
occupied-by-legal-persons, occupied-by-natural-persons, and unoccupied.
_Avoid_: Report, document, file

**Edition**:
The set of lists published on one date. RA-APPS publishes an edition on 10 June
and 10 December each year. Each edition is the unit of archival.
_Avoid_: Version, release, snapshot

**Ocupat / Neocupat**:
Whether an imobil is in use by a third party (ocupat) or vacant — free, in
litigation, in renovation, or proposed for sale/lease (neocupat).
_Avoid_: Occupied/vacant in mixed-language field values; rented (rent is one of several occupation types)

**Beneficiar**:
The third party using an ocupat imobil. Either a persoană juridică (legal entity)
or a persoană fizică (natural person, name redacted under GDPR).
_Avoid_: Tenant, lessee (the relationship is not always a lease)

**HG 420/2026**:
The 2026 government decision (amending HG 60/2005, art. 6^1) that mandates RA-APPS
to publish the lists. The legal basis and the reason the data exists at all.
