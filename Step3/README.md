# 🐳 Ansible Playbook: Creazione Registry e Container (Docker / Podman)

Questo playbook Ansible crea un registry locale e avvia due container utilizzando Docker o Podman, in modo parametrizzato e senza conflitto di porte.

---

## 📦 Requisiti

### Macchina di controllo (dove esegui Ansible)

- Python 3
- Ansible ≥ 2.12
- Collections richieste:
  - `community.docker`
  - `containers.podman`

## Macchina target (host rocky)

- **Rocky Linux o compatibile**
- **Docker oppure Podman installato**

## Variabili configurabili

|        Variabile       | Descrizione                         |  Default                                   |
| ---------------------- |:-----------------------------------:| ------------------------------------------:|
| container_runtime      | Motore container: docker o podman   | docker                                     |
| first_container_name   | Nome del primo container            | ubuntu                                     |
| first_container_image  | mmagine del primo container         | alessandrotofani/track3_step2:2.1          |
| first_container_port   | Porta host per il primo container   | 4797                                       |
| second_container_name  | Nome del secondo container          | rocky                                      |
| second_container_image | Immagine del secondo container      | alessandrotofani/track3_step2:1.1          |
| second_container_port  | Porta host per il secondo container | 4536                                       |

