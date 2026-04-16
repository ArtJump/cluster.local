# 💾 Хранилище (Local Path Provisioner)

Rancher Local Path Provisioner для динамического выделения `PersistentVolumes` на локальных дисках нод.

## 💿 Storage Classes

Доступно два независимых класса хранилища:

| StorageClass | По умолчанию | Reclaim Policy | Базовый путь | Назначение |
| :--- | :---: | :---: | :--- | :--- |
| `ssd` | ✅ Да | `Delete` | `/data/ssd` | Быстрые диски (БД, кэши) |
| `hdd` | ❌ Нет | `Retain` | `/data/hdd` | Медленные диски (файлопомойки) |

## 🛠 Подготовка ноды

Перед деплоем необходимо создать директории на сервере:

```bash
sudo mkdir -p /data/ssd
sudo chmod 777 /data/ssd

sudo mkdir -p /data/hdd
sudo chmod 777 /data/hdd
```

## 🧩 Файлы

* `namespace.yaml` — Namespace `local-path-storage`.
* `rbac.yaml` — Права доступа (ServiceAccount, ClusterRole, Role).
* `ssd-provisioner.yaml` — Deployment, StorageClass и ConfigMap для SSD.
* `hdd-provisioner.yaml` — Deployment, StorageClass и ConfigMap для HDD.

## 💡 Использование

По умолчанию PVC создаются на SSD. Для использования HDD укажите класс явно:

```yaml
kind: PersistentVolumeClaim
apiVersion: v1
metadata:
  name: torrent-data
spec:
  accessModes:
    - ReadWriteOnce
  storageClassName: hdd
  resources:
    requests:
      storage: 50Gi
```