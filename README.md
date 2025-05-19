## 📜 Licenca  
Ta dela so prosto dostopna za vsako uporabo brez omejitev.  
Avtor ne zahteva atribucije, vendar je hvaležen za povratne informacije.
___

## Za našo hišo je takšna maksimalno dovoljene dogovorjene moči (kW)!

![Sprememba dogovorjene moči](https://github.com/user-attachments/assets/f3fd7e9b-f6cb-4a89-8c5b-d751f2365208)

### 🤔 Treba je razumeti "politiko" elektra.

### Pri spremembi dogovorjene moči je potrebno upoštevati pravilo, `da mora biti dogovorjena moč v višjem časovnem bloku enaka ali večja kot v predhodnem`. 
To poenostavljeno pomeni, da npr. `dogovorjena moč v časovnem bloku 2 ne more biti manjša kot v časovnem bloku 1`.?

Taka "politika" nam ravno ne dovoljuje biti povsem kreativen a bom vseeno poiskusil.

***

🟥 Primer dejanske dosežene moči za mesec marec.
![202503-Moj elektro](https://github.com/user-attachments/assets/94b535d9-05b6-4e3c-936e-6e9eb7263825)

🟥 Primer dejanske dosežene moči za mesec april.
![Dosežene moči-04](https://github.com/user-attachments/assets/1aa5b09b-47e0-4725-b9cd-132c01444ae6)


Iz zgornjega je razvidno, da povprečje določeno s strani elektra odstopa od dejanskega, ki bi ga lahko upoštevali kot 15 minutno povprečje. 
Poglejte si razliko med prejeto delovno močjo in dogovorjenomočjo!
Tudi zakaj elektro za branje števca uporablja nezanesljivo tehnologijo branja števca mi nihče ni znal pojasniti, tako prihaja do izpada podatkov, ki jih števec po ponovni vzpostavitvi ne zna posalati (moja ocena je, da števec ne hrani dovolj dolgo preteklih podatkov)
Torej ima elektro dejanske podatke porabe in če bi hoteli zaračunavati pošteno po porabi bi to lahko storili. To je moje osebno mnenje, . 

___
Kako pa so opravili meritve za izračun dogovorjene moči?
![Meritve za izračun dogovorjene moči](https://github.com/user-attachments/assets/42b03b63-1a09-4ef4-9d03-7386b534ef89)

Kot je iz zgornje slike razvidno so vzeli tudi najvišje podatke porabe moči iz leta 2023 kot relevantne!?
Ne pozabimo, da te opravljene meritve oziroma DOGOVORJENA MOČ VELJAJO ZA CELO LETO, ki nam ki se ogrevamo (IR paneli) s pomočjo elektrike ne omogoča kaj dosti prilagajanja razen, če to prilagoditev sami zahtevamo!

![Faktor obračuna prekoračitve dogovorjene moči](https://github.com/user-attachments/assets/9b368e62-e148-4b7c-9cff-54419446c939)
Zgodilo se nam je že, da smo presegli dogovorjeno moč in ker je določeba s strani Elektra smo dobili samo pisno v obliki informacije.

Iz zgornje slike se da razbrati koliko moči je bilo preseženo in da je cena za obračun enaka ceni za normalno porabo s tem, da je povečana (pomnožite) za fakto 0,9, da dobite končni znesek!
Čisto moj razmislek mislim, da če bi sam zmanjšal dogovorjeno moč, ki bi se odražala v vsakomesečnem znesku in če bi slučajno presegel iz moje strani dogovorjeno moč, kazen nebi bila tako huda!?

___
Poglejmo si dva primera (moj dobavitelj je `Elektro energija, podjetje za prodajo elektrike in drugih energentov, svetovanje in storitve, d.o.o.`):

Mesec november 2024 (Omilitev draginje pri oskrbi z elektriko)
![2024-11_Stran_2](https://github.com/user-attachments/assets/f3d98f84-52af-4786-965c-378433d64093)

Mesec marec 2025
![202503_Stran_2](https://github.com/user-attachments/assets/a36743f0-ca2f-4ead-b552-fcfa4c2a3343)

Mesec april 2025
![Izračun-04](https://github.com/user-attachments/assets/6549f273-6f55-4020-ac59-8fa1187381b0)
Problem ko elektro ne prejme podatke lahko zakonsko uporabijo povprečje za izračun.
___
# 🎯 Pojasniti vam moram, da živimo v dvo družinski hiši kjer imamo trenutno 1 odjemno merilno mesto!
___

## Nekaj statistike in primerjave

Na spletni strani mojega elektra lahko po 1 dneh vidim, če vse deluje kot je za pričakovati:
![20250414-Povprečje 15 minut-ELEKTRO](https://github.com/user-attachments/assets/c7fec68a-ef59-4c2a-b494-2ce900f5d769)


In v Home assistant, kar ni popolno zato bom razložil v nadaljevanju kao se najbolj približati:
![20250414-Povprečje 15 minut](https://github.com/user-attachments/assets/4e08c265-52c7-46f0-9800-cbc4963675a7)

## Da v Home assistant sproti vidim nekaj približno podobnega sem moral narediti nekaj novih entitet:

Če ste do sedaj že spremljali moji objavi:

https://github.com/Trzinka/Home_Assistant-Obracun_porabe_elektricne_energije_po_novem-2025 in 

https://github.com/Trzinka/Home_Assistant-Obracun_porabe_elektricne_energije_po_novem-2025_Prikaz_porabe_za_prejsnji_mesec

potem poznate strukturo mojega korenskega imenika.
___

✍️ Torej v mapi `share` in v njeni podmapi `sensors` naredite datoteko `15_minut.yaml` in v njo dodajte:
```yaml
- platform: statistics
  name: "Povprečna moč (15 min)-skupaj"
  unique_id: sensor_povprecna_moc_15min_skupaj
  entity_id: sensor.p1_meter_power
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "Povprečna moč (15 min)-faza 1"
  unique_id: sensor_povprecna_moc_15min_faza1
  entity_id: sensor.p1_meter_power_phase_1
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "Povprečna moč (15 min)-faza 2"
  unique_id: sensor_povprecna_moc_15min_faza2
  entity_id: sensor.p1_meter_power_phase_2
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "Povprečna moč (15 min)-faza 3"
  unique_id: sensor_povprecna_moc_15min_faza3
  entity_id: sensor.p1_meter_power_phase_3
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300
#################################################
- platform: statistics
  name: "ME BO - Povprečna moč (15 min)"
  unique_id: sensor_me_bo_avg_power_15min
  entity_id: sensor.me_bo_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "ME PRST - Povprečna moč (15 min)"
  unique_id: sensor_me_prst_avg_power_15min
  entity_id: sensor.me_prst_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "ME SS - Povprečna moč (15 min)"
  unique_id: sensor_me_ss_avg_power_15min
  entity_id: sensor.me_ss_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "ME KL - Povprečna moč (15 min)"
  unique_id: sensor_me_kl_avg_power_15min
  entity_id: sensor.me_kl_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300

- platform: statistics
  name: "TM DI - Povprečna moč (15 min)"
  entity_id: sensor.tm_di_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300
  unique_id: b6911e7b-49ec-40f5-ad48-dba6d604b9c6

- platform: statistics
  name: "TM NA - Povprečna moč (15 min)"
  entity_id: sensor.tm_na_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300
  unique_id: 523d8e47-2812-4e37-8325-431ca9d2f9d2

- platform: statistics
  name: "TM SP - Povprečna moč (15 min)"
  entity_id: sensor.tm_sp_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300
  unique_id: 45c0a4eb-749a-4600-939d-4c4c16756525

- platform: statistics
  name: "ME JE POD KLIMO - Povprečna moč (15 min)"
  entity_id: sensor.me_je_pod_klimo_current_consumption
  state_characteristic: mean
  max_age:
    minutes: 15
  sampling_size: 300
  unique_id: 3ef4acea-93ed-4745-9904-a2b125f4e520
```
s tem ste ustvarili entitete, ki v časovnem obdobju 15 minut izračuna povprečje porabe v W
___

✍️ Sedaj je potrebno v datoteki `template.yaml` v korenskem imeniku vpisati:
```yaml
- sensor:
  - name: "Povprečna moč 15 min (kW)-Skupaj"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.povprecna_moc_15_min_skupaj') | float(0) / 1000) | round(2) }}
    unique_id: 6b269620-acc4-46b9-851e-823163bb96cd

  - name: "Povprečna moč 15 min (kW)-Faza 1"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.povprecna_moc_15_min_faza_1') | float(0) / 1000) | round(2) }}
    unique_id: 84a65eba-4ef0-4777-b85c-9b578e987b97

  - name: "Povprečna moč 15 min (kW)-Faza 2"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.povprecna_moc_15_min_faza_2') | float(0) / 1000) | round(2) }}
    unique_id: aac240b8-db54-4767-b807-5a00cd29ac5b

  - name: "Povprečna moč 15 min (kW)-Faza 3"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.povprecna_moc_15_min_faza_3') | float(0) / 1000) | round(2) }}
    unique_id: 604cc278-4b73-48e6-9556-ec9a17f4a7bf
################################################################################################################
- sensor:
  - name: "ME BO - Povprečna moč 15 min (kW)"
    unique_id: sensor_me_bo_avg_power_15min_kw
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.me_bo_povprecna_moc_15_min') | float(0) / 1000) | round(2) }}

  - name: "ME PRST - Povprečna moč 15 min (kW)"
    unique_id: sensor_me_prst_avg_power_15min_kw
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.me_prst_povprecna_moc_15_min') | float(0) / 1000) | round(2) }}

  - name: "ME SS - Povprečna moč 15 min (kW)"
    unique_id: sensor_me_ss_avg_power_15min_kw
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.me_ss_povprecna_moc_15_min') | float(0) / 1000) | round(2) }}

  - name: "ME KL - Povprečna moč 15 min (kW)"
    unique_id: sensor_me_kl_avg_power_15min_kw
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.me_kl_povprecna_moc_15_min') | float(0) / 1000) | round(2) }}

  - name: "TM DI - Povprečna moč 15 min (kW)"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.tm_di_povprecna_moc_15_min') | float(0) / 1000) | round(2) }} 
    unique_id: 972853ae-c019-41e8-8ba8-d1501cd54f15

  - name: "TM NA - Povprečna moč 15 min (kW)"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.tm_na_povprecna_moc_15_min') | float(0) / 1000) | round(2) }} 
    unique_id: 4ec94641-d1bd-468a-be71-52488353e287

  - name: "TM SP - Povprečna moč 15 min (kW)"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.tm_sp_povprecna_moc_15_min') | float(0) / 1000) | round(2) }} 
    unique_id: 831e2ca6-37cd-4289-a2af-4faf07a3dab9

  - name: "ME JE POD KLIMO - Povprečna moč 15 min (kW)"
    unit_of_measurement: "kW"
    state_class: measurement
    device_class: power
    state: >
      {{ (states('sensor.me_je_pod_klimo_povprecna_moc_15_min') | float(0) / 1000) | round(2) }} 
    unique_id: 06520c72-9a7f-4743-bcb8-824f10e51595
```
S tem naredite entitete, ki merijo porabo v Kw
____

✍️ in še v `automations.yaml`
```yaml
- id: posodobi_15_min_template_senzorje
  alias: Posodobi 15-minutne template senzorje
  triggers:
    - platform: time_pattern
      minutes: "/15"
  actions:
    - service: homeassistant.update_entity
      target:
        entity_id:
          - sensor.povprecna_moc_15_min_kw_skupaj
          - sensor.povprecna_moc_15_min_kw_faza_1
          - sensor.povprecna_moc_15_min_kw_faza_2
          - sensor.povprecna_moc_15_min_kw_faza_3
          - sensor.tm_na_povprecna_moc_15_min
          - sensor.tm_di_povprecna_moc_15_min
          - sensor.tm_sp_povprecna_moc_15_min
          - sensor.me_ss_avg_power_15min_kw
          - sensor.me_bo_avg_power_15min_kw
          - sensor.me_kl_avg_power_15min_kw
          - sensor.me_prst_avg_power_15min_kw
          - sensor.me_je_pod_klimo_povprecna_moc_15_min
  mode: single
```
s tem ustvarimo avtomatizacijo entitete zbiranja na 15 minut, ki jo uporabljam v grafu (zgodovinskem izpisu). 
Opazili boste, da prikazani podatki niso ravno na 15 minut, a so dovolj blizu za uporabo!

***


## 🧐 Čeprav bi 15 minutno povprečje moralo izgledati nekaj podobno temu, ker se primerjalni rezultat z Elektrom najbolj približa:
![20250414-Snapshot-Povprečje 15 minut](https://github.com/user-attachments/assets/5d8ee82c-8a1c-4002-b7f0-3ab7df0a71ac)

![20250414-Povprečje 15 minut-ELEKTRO](https://github.com/user-attachments/assets/e24c1654-b353-4697-b013-b6e72ac4b94b)


### ✍️ Da bi dobili podatke kot jih prikazuje zgornja slika v datoteki `template.yaml` v korenskem imeniku vpišite:
```yaml
##################################################################################################################
- sensor:
  - name: "Snapshot Povprečna moč 15 min (kW) - Skupaj"
    unique_id: sensor_snapshot_povprecna_moc_15_min_kw_skupaj
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_povprecna_moc_15_min_kw_skupaj') | float(0) }}

  - name: "Snapshot Povprečna moč 15 min (kW) - Faza 1"
    unique_id: sensor_snapshot_povprecna_moc_15_min_kw_faza_1
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_povprecna_moc_15_min_kw_faza_1') | float(0) }}

  - name: "Snapshot Povprečna moč 15 min (kW) - Faza 2"
    unique_id: sensor_snapshot_povprecna_moc_15_min_kw_faza_2
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_povprecna_moc_15_min_kw_faza_2') | float(0) }}

  - name: "Snapshot Povprečna moč 15 min (kW) - Faza 3"
    unique_id: sensor_snapshot_povprecna_moc_15_min_kw_faza_3
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_povprecna_moc_15_min_kw_faza_3') | float(0) }}

  - name: "Snapshot TM NA - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_tm_na_povprecna_moc_15_min
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_tm_na_povprecna_moc_15_min') | float(0) }}

  - name: "Snapshot TM DI - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_tm_di_povprecna_moc_15_min
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_tm_di_povprecna_moc_15_min') | float(0) }}

  - name: "Snapshot TM SP - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_tm_sp_povprecna_moc_15_min
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_tm_sp_povprecna_moc_15_min') | float(0) }}

  - name: "Snapshot ME SS - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_me_ss_avg_power_15min_kw
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_me_ss_avg_power_15min_kw') | float(0) }}

  - name: "Snapshot ME BO - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_me_bo_avg_power_15min_kw
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_me_bo_avg_power_15min_kw') | float(0) }}

  - name: "Snapshot ME KL - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_me_kl_avg_power_15min_kw
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_me_kl_avg_power_15min_kw') | float(0) }}

  - name: "Snapshot ME PRST - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_me_prst_avg_power_15min_kw
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_me_prst_avg_power_15min_kw') | float(0) }}

  - name: "Snapshot ME JE POD KLIMO - Povprečna moč 15 min (kW)"
    unique_id: sensor_snapshot_me_je_pod_klimo_povprecna_moc_15_min
    unit_of_measurement: "kW"
    device_class: power
    state_class: measurement
    state: >
      {{ states('input_number.snapshot_me_je_pod_klimo_povprecna_moc_15_min') | float(0) }}
```

✍️ Poleg tega je potrebno v `configuration.yaml` dodati:
```yaml
input_number:
  snapshot_povprecna_moc_15min_skupaj:
    name: Snapshot - Povprečna moč 15 min (kW) - Skupaj
    unit_of_measurement: "kW"
    min: 0
    max: 20
    step: 0.01

  snapshot_povprecna_moc_15min_faza_1:
    name: Snapshot - Povprečna moč 15 min (kW) - Faza 1
    unit_of_measurement: "kW"
    min: 0
    max: 20
    step: 0.01

  snapshot_povprecna_moc_15min_faza_2:
    name: Snapshot - Povprečna moč 15 min (kW) - Faza 2
    unit_of_measurement: "kW"
    min: 0
    max: 20
    step: 0.01

  snapshot_povprecna_moc_15min_faza_3:
    name: Snapshot - Povprečna moč 15 min (kW) - Faza 3
    unit_of_measurement: "kW"
    min: 0
    max: 20
    step: 0.01

  snapshot_me_bo:
    name: Snapshot - ME BO
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_me_prst:
    name: Snapshot - ME PRST
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_me_ss:
    name: Snapshot - ME SS
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_me_kl:
    name: Snapshot - ME KL
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_tm_di:
    name: Snapshot - TM DI
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_tm_na:
    name: Snapshot - TM NA
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_tm_sp:
    name: Snapshot - TM SP
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01

  snapshot_me_je_pod_klimo:
    name: Snapshot - ME JE POD KLIMO
    unit_of_measurement: "kW"
    min: 0
    max: 10
    step: 0.01
```

✍️ in še v `automations.yaml`
```yaml
- id: shrani_15_min_snapshot
  alias: Shrani 15-minutni snapshot povprečne moči
  trigger:
    - platform: time_pattern
      minutes: "/15"
  action:
    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_povprecna_moc_15min_skupaj
      data:
        value: "{{ states('sensor.povprecna_moc_15_min_kw_skupaj') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_povprecna_moc_15min_faza_1
      data:
        value: "{{ states('sensor.povprecna_moc_15_min_kw_faza_1') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_povprecna_moc_15min_faza_2
      data:
        value: "{{ states('sensor.povprecna_moc_15_min_kw_faza_2') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_povprecna_moc_15min_faza_3
      data:
        value: "{{ states('sensor.povprecna_moc_15_min_kw_faza_3') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_me_bo
      data:
        value: "{{ states('sensor.me_bo_avg_power_15min_kw') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_me_prst
      data:
        value: "{{ states('sensor.me_prst_avg_power_15min_kw') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_me_ss
      data:
        value: "{{ states('sensor.me_ss_avg_power_15min_kw') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_me_kl
      data:
        value: "{{ states('sensor.me_kl_avg_power_15min_kw') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_tm_di
      data:
        value: "{{ states('sensor.tm_di_povprecna_moc_15_min') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_tm_na
      data:
        value: "{{ states('sensor.tm_na_povprecna_moc_15_min') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_tm_sp
      data:
        value: "{{ states('sensor.tm_sp_povprecna_moc_15_min') | float(0) }}"

    - service: input_number.set_value
      target:
        entity_id: input_number.snapshot_me_je_pod_klimo
      data:
        value: "{{ states('sensor.me_je_pod_klimo_povprecna_moc_15_min') | float(0) }}"
  mode: single
```

***

### Slika prikazuje razliko v prikazu/podatkih porabe med prvimi 15 minutnimi entitetami in kasneje dodelanimi entitetami, ki so bližje prikazu/podatkom Mojega elektra:
![Poraba od 14 do 20 04 2025](https://github.com/user-attachments/assets/f8a83c1c-ffaf-4ffa-8a8d-c225ce593639)
