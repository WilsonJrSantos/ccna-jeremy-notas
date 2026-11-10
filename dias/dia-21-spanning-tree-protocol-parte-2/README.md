# Día 21 · Spanning Tree Protocol (parte 2)

> Mis apuntes del Día 21 del curso **CCNA 200-301 de Jeremy's IT Lab**.
> Video del día: [Jeremy's IT Lab en YouTube](https://www.youtube.com/@JeremysITLab).

| | |
|---|---|
| **Estado** | ✅ Estudiado |
| **Lectura Odom 2.ª ed.** | V1 caps. 9–10 |
| **NetAcad v7** | SRWE 5 |
| **Lab extra** | LG 25-2 STP |

## Diapositivas clave de Jeremy

**21-1 Slides - PortFast**

<table><tr><td width="50%"><img src="img/diapositivas/d21_21_2.jpg" width="100%"><br><sub>p. 2</sub></td><td width="50%"><img src="img/diapositivas/d21_21_4.jpg" width="100%"><br><sub>p. 4</sub></td></tr><tr><td width="50%"><img src="img/diapositivas/d21_21_9.jpg" width="100%"><br><sub>p. 9</sub></td></tr></table>

**21-2 Slides - BPDU Guard & BPDU Filter**

<table><tr><td width="50%"><img src="img/diapositivas/d21_22_7.jpg" width="100%"><br><sub>p. 7</sub></td><td width="50%"><img src="img/diapositivas/d21_22_8.jpg" width="100%"><br><sub>p. 8</sub></td></tr><tr><td width="50%"><img src="img/diapositivas/d21_22_12.jpg" width="100%"><br><sub>p. 12</sub></td><td width="50%"><img src="img/diapositivas/d21_22_14.jpg" width="100%"><br><sub>p. 14</sub></td></tr><tr><td width="50%"><img src="img/diapositivas/d21_22_16.jpg" width="100%"><br><sub>p. 16</sub></td></tr></table>

**21-3 Slides - Root Guard**

<table><tr><td width="50%"><img src="img/diapositivas/d21_23_5.jpg" width="100%"><br><sub>p. 5</sub></td><td width="50%"><img src="img/diapositivas/d21_23_7.jpg" width="100%"><br><sub>p. 7</sub></td></tr><tr><td width="50%"><img src="img/diapositivas/d21_23_13.jpg" width="100%"><br><sub>p. 13</sub></td></tr></table>

**21-4 Slides - Loop Guard**

<table><tr><td width="50%"><img src="img/diapositivas/d21_24_8.jpg" width="100%"><br><sub>p. 8</sub></td><td width="50%"><img src="img/diapositivas/d21_24_9.jpg" width="100%"><br><sub>p. 9</sub></td></tr><tr><td width="50%"><img src="img/diapositivas/d21_24_14.jpg" width="100%"><br><sub>p. 14</sub></td><td width="50%"><img src="img/diapositivas/d21_24_15.jpg" width="100%"><br><sub>p. 15</sub></td></tr></table>

**Day 21 Slides - STP (Part 2)**

<table><tr><td width="50%"><img src="img/diapositivas/d21_25_8.jpg" width="100%"><br><sub>p. 8</sub></td><td width="50%"><img src="img/diapositivas/d21_25_13.jpg" width="100%"><br><sub>p. 13</sub></td></tr><tr><td width="50%"><img src="img/diapositivas/d21_25_16.jpg" width="100%"><br><sub>p. 16</sub></td><td width="50%"><img src="img/diapositivas/d21_25_19.jpg" width="100%"><br><sub>p. 19</sub></td></tr><tr><td width="50%"><img src="img/diapositivas/d21_25_24.jpg" width="100%"><br><sub>p. 24</sub></td><td width="50%"><img src="img/diapositivas/d21_25_25.jpg" width="100%"><br><sub>p. 25</sub></td></tr><tr><td width="50%"><img src="img/diapositivas/d21_25_28.jpg" width="100%"><br><sub>p. 28</sub></td><td width="50%"><img src="img/diapositivas/d21_25_29.jpg" width="100%"><br><sub>p. 29</sub></td></tr><tr><td width="50%"><img src="img/diapositivas/d21_25_35.jpg" width="100%"><br><sub>p. 35</sub></td><td width="50%"><img src="img/diapositivas/d21_25_36.jpg" width="100%"><br><sub>p. 36</sub></td></tr><tr><td width="50%"><img src="img/diapositivas/d21_25_38.jpg" width="100%"><br><sub>p. 38</sub></td><td width="50%"><img src="img/diapositivas/d21_25_42.jpg" width="100%"><br><sub>p. 42</sub></td></tr></table>


## Apuntes

**Estados de los puertos en STP clásico (802.1D):**

| Estado | ¿Envía/recibe BPDU? | ¿Aprende MAC? | ¿Reenvía datos? | Duración |
|---|---|---|---|---|
| **Blocking** | Recibe | No | No | Estable (non-designated) |
| **Listening** | Sí | **No** | No | **15 s** (forward delay) |
| **Learning** | Sí | **Sí** | No | **15 s** (forward delay) |
| **Forwarding** | Sí | Sí | **Sí** | Estable (root/designated) |
| Disabled | — | — | — | `shutdown` |

**Temporizadores:** Hello **2 s**, Forward delay **15 s**, Max age **20 s** (10 × hello). Un puerto bloqueado que deja de recibir BPDU espera **max age (20) + listening (15) + learning (15) = 50 s** antes de reenviar.

**Formato de la BPDU:** incluye Root Bridge ID, Root Path Cost, Bridge ID del emisor, Port ID y los temporizadores. Destino MAC **0180.C200.0000** (STP) o **0100.0CCC.CCCD** (PVST+ de Cisco).

**Versiones:** 802.1D (STP, una instancia para todas las VLAN) → **PVST+** (Cisco, una instancia **por VLAN**, permite balancear carga) → 802.1w (RSTP) → **Rapid PVST+** (Cisco) → 802.1s (MSTP, agrupa VLAN en instancias).

**Comandos clave:**

```
SW1(config)# spanning-tree vlan 1 root primary         ← pone prioridad 24576 (o 4096 menos que el root actual)
SW1(config)# spanning-tree vlan 2 root secondary       ← pone prioridad 28672
SW1(config)# spanning-tree vlan 1 priority 4096        ← manual, múltiplo de 4096
SW1(config-if)# spanning-tree vlan 1 cost 100
SW1(config-if)# spanning-tree vlan 1 port-priority 240 ← múltiplos de 16 (0–240), por defecto 128
SW1# show spanning-tree vlan 1
```

**Funciones opcionales (las "guards"):**

| Función | Dónde se pone | Qué hace |
|---|---|---|
| **PortFast** | Puertos hacia hosts | Pasa a forwarding **de inmediato** (salta listening/learning). `spanning-tree portfast` o `spanning-tree portfast default` (global) |
| **BPDU Guard** | Puertos PortFast | Si llega una BPDU → **err-disabled**. `spanning-tree bpduguard enable` |
| **BPDU Filter** | Puertos PortFast | No envía BPDU; si recibe una, deja de ser PortFast (global) o ignora BPDU (por interfaz) |
| **Root Guard** | Puertos que **no** deben llevar al root | Si recibe una BPDU superior → *broken (root inconsistent)* |
| **Loop Guard** | Puertos no designated | Si dejan de llegar BPDU, no pasa a forwarding → *loop inconsistent* |

Recuperar un puerto err-disabled: `shutdown` + `no shutdown`, o `errdisable recovery cause bpduguard`.

## Trampas de examen

1. STP clásico tarda **30 s** de blocking a forwarding tras un cambio directo (15+15), o **50 s** con max age.
2. `root primary` no fija 0: pone 24576 o un valor por debajo del root actual.
3. La prioridad de puerto solo **desempata**; nunca gana a un costo menor.
4. PortFast en un puerto hacia un switch puede causar bucles: siempre con BPDU Guard.

## Laboratorio · Configurar STP


<p align="center"><img src="img/lab/21.png" width="85%"><br><sub>Pantallazo del laboratorio (enunciado original de Jeremy)</sub></p>


**Topología:** SW1 F0/1–SW3 F0/1; SW1 F0/2–SW4 F0/2; SW1 F0/3–SW2 F0/3; SW2 F0/1–SW4 F0/1; SW2 F0/2–SW3 F0/2. PC1 en SW3 F0/3 (VLAN1, 172.16.0.0/25). PC2 en SW4 F0/3 (VLAN2, 172.16.0.128/25). Todos los enlaces entre switches son trunks y la VLAN 2 existe en los cuatro.

**Tarea 1 · Ver la topología actual: root bridge y rol/estado de cada puerto.**

```
SW1# show spanning-tree vlan 1
SW1# show spanning-tree vlan 2
```

Con prioridades por defecto, el root es el switch con **MAC más baja** (compara las MAC que muestra "Bridge ID" en cada switch). Anota los roles para comparar luego.

**Tarea 2 · SW1 root primario VLAN1 y secundario VLAN2; SW2 root primario VLAN2 y secundario VLAN1. ¿Roles ahora?**

```
SW1(config)# spanning-tree vlan 1 root primary
SW1(config)# spanning-tree vlan 2 root secondary
SW2(config)# spanning-tree vlan 2 root primary
SW2(config)# spanning-tree vlan 1 root secondary
```

| VLAN 1 (root SW1) | Rol |
|---|---|
| SW1 F0/1, F0/2, F0/3 | Designated |
| SW2 F0/3 | Root port |
| SW2 F0/1, F0/2 | Designated (SW2 es secundario: BID menor que SW3/SW4 con el mismo costo 19) |
| SW3 F0/1 | Root port · SW3 F0/2 **bloqueado** · SW3 F0/3 designated |
| SW4 F0/2 | Root port · SW4 F0/1 **bloqueado** · SW4 F0/3 designated |

| VLAN 2 (root SW2) | Rol |
|---|---|
| SW2 F0/1, F0/2, F0/3 | Designated |
| SW1 F0/3 | Root port · SW1 F0/1, F0/2 designated (SW1 es secundario) |
| SW3 F0/2 | Root port · SW3 F0/1 **bloqueado** · F0/3 designated |
| SW4 F0/1 | Root port · SW4 F0/2 **bloqueado** · F0/3 designated |

**Tarea 3 · Costo VLAN1 de SW4 F0/2 = 100. ¿SW4 elige otro root port?**

```
SW4(config)# interface f0/2
SW4(config-if)# spanning-tree vlan 1 cost 100
```

**Sí.** Por F0/2 el costo ahora es 100; por F0/1 (vía SW2) es 19 + 19 = **38**. El root port pasa a **F0/1** y F0/2 queda bloqueado.

**Tarea 4 · Prioridad VLAN1 de SW1 F0/1 = 240. ¿SW3 elige otro root port?**

```
SW1(config)# interface f0/1
SW1(config-if)# spanning-tree vlan 1 port-priority 240
```

**No.** El camino directo de SW3 (F0/1) cuesta 19 y el alternativo por SW2 cuesta 38. La prioridad de puerto del vecino solo cuenta cuando **costo y BID del vecino empatan**.

**Tarea 5 · PortFast y BPDU Guard en F0/3 de SW3 y SW4.**

```
SW3(config)# interface f0/3
SW3(config-if)# spanning-tree portfast
SW3(config-if)# spanning-tree bpduguard enable
SW4(config)# interface f0/3
SW4(config-if)# spanning-tree portfast
SW4(config-if)# spanning-tree bpduguard enable
```

---

## Rómpelo y arréglalo (48 h después del lab)

| # | Rompe esto | Síntoma | Te delata | Arreglo |
|---|---|---|---|---|
| 1 | Conecta un switch nuevo a SW3 F0/3 (BPDU Guard) | El puerto se apaga | `show interfaces status` → err-disabled; log `BPDUGUARD` | Quitar el switch, `shutdown`/`no shutdown` |
| 2 | Olvida crear la VLAN 2 en SW3 | PC2 bien, pero STP de VLAN 2 incompleto en SW3 | `show spanning-tree vlan 2` → no existe | Crear VLAN 2 |
| 3 | SW2 `spanning-tree vlan 1 priority 0` | SW2 se vuelve root de VLAN 1 | `show spanning-tree root` | Quitar el comando |

## Repaso rápido

Intenta responder antes de abrir cada pregunta.

<details><summary>Estados de STP clásico y sus tiempos</summary>

Blocking, Listening (15 s), Learning (15 s), Forwarding.

</details>

<details><summary>Temporizadores STP por defecto</summary>

Hello 2 s, forward delay 15 s, max age 20 s.

</details>

<details><summary>¿Qué hace PortFast?</summary>

Pasa el puerto a forwarding al instante (solo hacia hosts).

</details>

<details><summary>¿Qué hace BPDU Guard al recibir una BPDU?</summary>

Pone el puerto en err-disabled.

</details>

<details><summary>Root Guard vs Loop Guard</summary>

Root Guard: BPDU superior → root-inconsistent. Loop Guard: dejan de llegar BPDU → loop-inconsistent.

</details>

<details><summary>¿Qué prioridad pone spanning-tree vlan X root primary?</summary>

24576 (o 4096 menos que el root actual).

</details>

<details><summary>¿Cuándo importa la port-priority?</summary>

Solo para desempatar cuando costo y BID del vecino son iguales.

</details>


---

[⬅ Índice del curso](../../README.md)
