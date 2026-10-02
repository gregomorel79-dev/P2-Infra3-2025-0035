# P2 – Infra 3: VPN de acceso remoto (Dialup) FortiGate + MikroTik
**Gregorys Morel Duluc – 2025-0035 – Seguridad de Redes (ITLA)**
https://youtu.be/j5v7lGj5dFg
## Topología
Cliente remoto PC3 (detrás de MT1) ⇄ ISP 200.35.0.0/24 ⇄ FG1 (servidor de VPN) ⇄ servidor WEB3.

## Plan de IPs
| Equipo | WAN | LAN |
|---|---|---|
| FG1 | 200.35.0.1/24 | 10.0.35.129/25 |
| WEB3 (HTTPS + SSH) | — | 10.0.35.250/25 |
| MT1 | 200.35.0.3/24 | 10.0.36.1/24 |
| PC3 (cliente) | — | 10.0.36.10/24 |

## Configuración
- FG1 (GUI): túnel `VPN-REMOTO` tipo Dialup User (IKEv1, DES/SHA1, DH14).
- VIP `200.35.0.1:8443 → 10.0.35.250:443` + política `WEB-PUBLICO` (HTTPS sin VPN).
- Política `VPN-SSH`: solo SSH desde la red remota a través del túnel.
- MT1: cliente IPsec hacia FG1 y NAT de salida.

## Pruebas
- Sin VPN: `curl -k https://200.35.0.1:8443` funciona; `ssh root@10.0.35.250` falla.
- Con VPN: `ssh root@10.0.35.250` entra.

## Archivos
`FG1-Infra3.conf`, `MT1-Infra3.rsc`
