# Топ-20 популярных легитимных утилит удалённого доступа под Windows (в злоупотреблениях 2020–2025)

## Что было сделано

1. Разобрана структура репозитория:
   - `yaml/` — карточки инструментов (метаданные, артефакты, индикаторы, иногда пути к настройкам).
   - `detections/sigma/` — Sigma-правила детектирования активности инструментов.
   - вспомогательно: `detections/kql/`, `detections/splunk/`.
2. Сформирован пул кандидатов из `yaml/`.
3. Проведена **внешняя** оценка популярности (последние 5 лет), с опорой прежде всего на:
   - публичные IR/Threat Intel отчёты,
   - MITRE ATT&CK (T1219 Remote Access Software),
   - практики red team/pentest.
4. Для каждой утилиты добавлены:
   - классификация: `bind` / `через промежуточный сервер` / `гибрид`,
   - способ хранения настроек в Windows (реестр/файлы),
   - пути (или честная пометка о недостаточной надёжности).

> Важно: точные «абсолютные» количества упоминаний по каждому вендору/группе зачастую не публикуются в нормализованном виде. Поэтому в таблице используются **оценочные диапазоны** частоты и итоговый score.

## Метод ранжирования

Итоговый балл (`0..15`) =
- `IR/Threat Intel` (`0..5`) — как часто инструмент фигурирует в расследованиях/аналитике,
- `ATT&CK релевантность` (`0..5`) — применимость к T1219-процедурам,
- `Red Team / Pentest` (`0..5`) — частота практического использования легитимного инструмента как канала доступа.

Промежуточные данные:
- `01_candidates_from_repo.csv`
- `02_external_scoring_top20.csv`
- `03_windows_config_locations.csv`

---

## Top-20 (Windows-фокус)

| # | Утилита | Тип доступа | Популярность (оценка) | Где хранит настройки в Windows | Ссылка на проект | YAML в репо |
|---:|---|---|---|---|---|---|
| 1 | AnyDesk | Через промежуточный сервер | Очень часто (100+) | Реестр + `user.conf/system.conf/service.conf` (`%APPDATA%\AnyDesk`, `%PROGRAMDATA%\AnyDesk`) | https://anydesk.com/en | `yaml/anydesk.yaml` |
| 2 | ScreenConnect (ConnectWise Control) | Через промежуточный сервер | Очень часто (80+) | Файлы/БД (`App_Data\Session.db`, `User.xml`, `ProgramData\ScreenConnect Client*\user.config`) | https://www.screenconnect.com/ | `yaml/screenconnect.yaml`, `yaml/connectwise_control.yaml` |
| 3 | TeamViewer | Через промежуточный сервер | Очень часто (80+) | Реестр `HKLM/HKU\...\TeamViewer` + файлы/логи в `%APPDATA%`, `%LOCALAPPDATA%` | https://www.teamviewer.com/en | `yaml/teamviewer.yaml` |
| 4 | Splashtop | Через промежуточный сервер | Часто (50+) | Реестр Splashtop + файлы/БД в `Program Files (x86)\Splashtop\...\db/log` | https://www.splashtop.com/ | `yaml/splashtop.yaml` |
| 5 | NetSupport Manager | Через промежуточный сервер | Часто (50+) | Вероятно конфиги в каталоге NetSupport (INI/CFG), стабильные пути зависят от сборки | https://www.netsupportmanager.com/ | `yaml/netsupport_manager.yaml` |
| 6 | Atera | Через промежуточный сервер | Часто (40+) | Реестр `...\ATERA Networks\AlphaAgent` + файлы агента в `C:\Program Files\ATERA Networks\AteraAgent\*` | https://www.atera.com/ | `yaml/atera.yaml` |
| 7 | LogMeIn Rescue | Через промежуточный сервер | Часто (40+) | В основном пользовательские/временные файлы; точные стабильные пути зависят от режима applet/agent | https://www.logmein.com/products/rescue | `yaml/logmein_rescue.yaml` |
| 8 | GoToAssist / GoTo Resolve | Через промежуточный сервер | Часто (35+) | Реестр `HKLM\SOFTWARE\GoTo Resolve Unattended` + пользовательские кэши/логи (вариативно) | https://www.logmein.com/products/resolve | `yaml/gotoassist_(goto_resolve).yaml` |
| 9 | NinjaOne (NinjaRMM) | Через промежуточный сервер | Часто (30+) | Обычно агент в `C:\ProgramData\NinjaRMMAgent\*` (вариативно по версии/брендингу) | https://www.ninjaone.com/rmm/ | `yaml/ninjaone_(formerly_ninjarmm).yaml` |
| 10 | Remote Utilities | Гибрид | Умеренно часто (25+) | Реестр + файлы Host/Viewer; portable-режим часто хранит рядом с exe | https://www.remoteutilities.com/ | `yaml/remote_utilities.yaml` |
| 11 | RustDesk | Гибрид | Умеренно часто (20+) | Файлы в профиле пользователя/службы (`%APPDATA%\RustDesk`, `...LocalService\...\RustDesk`) | https://github.com/rustdesk/rustdesk | `yaml/rustdesk.yaml` |
| 12 | Chrome Remote Desktop | Через промежуточный сервер | Умеренно часто (20+) | Связанные данные в профиле Chrome + служба CRD; часть настроек аккаунтная | https://remotedesktop.google.com/ | `yaml/chrome_remote_desktop.yaml` |
| 13 | Action1 | Через промежуточный сервер | Умеренно часто (15+) | Реестр `...\Services\A1Agent`, `...\WOW6432Node\Action1` + `C:\Windows\Action1\*` | https://www.action1.com/ | `yaml/action1.yaml` |
| 14 | BeyondTrust Remote Support (Bomgar) | Через промежуточный сервер | Умеренно часто (15+) | Агентные файлы в ProgramData/Program Files; точные пути зависят от схемы развертывания | https://www.beyondtrust.com/products/remote-support | `yaml/beyondtrust_(bomgar).yaml` |
| 15 | Zoho Assist | Через промежуточный сервер | Умеренно часто (15+) | Обычно Program Files/ProgramData/AppData; точные пути зависят от unattended/ad-hoc режима | https://www.zoho.com/assist/ | `yaml/zohoassist.yaml` |
| 16 | Radmin | Bind | Умеренно часто (15+) | Реестр `HKLM\SOFTWARE\WOW6432Node\Radmin\...` + `C:\Windows\System32\rserver30\*` | https://www.radmin.com/ | `yaml/radmin.yaml` |
| 17 | UltraViewer | Через промежуточный сервер | Точечно (10+) | Обычно пользовательские файлы в `%APPDATA%\UltraViewer\*` (зависит от версии) | https://www.ultraviewer.net/ | `yaml/ultraviewer.yaml` |
| 18 | RealVNC Connect | Гибрид (bind/relay) | Точечно (10+) | Реестр + конфиги VNC Server/Viewer; ветки/пути зависят от редакции | https://www.realvnc.com/en/connect/ | `yaml/realvnc.yaml` |
| 19 | TightVNC | Bind | Точечно (10+) | Обычно `HKLM/HKCU\...\TightVNC` + ini в каталоге установки | https://www.tightvnc.com/ | `yaml/tightvnc.yaml` |
| 20 | UltraVNC | Bind | Точечно (10+) | Обычно `HKLM\SOFTWARE\UltraVNC` + `ultravnc.ini` в каталоге установки | https://uvnc.com/ | `yaml/ultra_vnc.yaml` |

---

## Ключевые наблюдения

1. **Доминирует модель “через промежуточный сервер”**: для злоумышленника это удобнее (NAT traversal, меньше сетевых изменений на цели).
2. **Bind-инструменты** (Radmin/VNC-семейство) по-прежнему используются, но чаще точечно и обычно требуют более «грубых» сетевых изменений (порт/правила/проброс).
3. **Самый сложный и важный момент для DFIR/детекта** — не факт наличия бинаря, а именно:
   - режим установки (portable vs service/unattended),
   - где лежат connection history / policy / unattended credentials,
   - что синхронизируется в облако и не остаётся локально в полном объёме.
4. Для части вендоров точные пути настроек **сильно зависят от версии и канала установки**. Это отражено в `03_windows_config_locations.csv` с уровнем надёжности.

## Внешние источники (база для оценки популярности)

- MITRE ATT&CK T1219: https://attack.mitre.org/techniques/T1219/
- CISA AA23-025A (RMM abuse): https://www.cisa.gov/news-events/cybersecurity-advisories/aa23-025a
- CISA AA24-241A (актуальные кампании): https://www.cisa.gov/news-events/cybersecurity-advisories/aa24-241a
- DFIR Report (пример ScreenConnect→Hive): https://thedfirreport.com/2023/09/25/from-screenconnect-to-hive-ransomware-in-61-hours/
- Synacktiv, forensics legitimate RAT/RMM: https://www.synacktiv.com/publications/legitimate-rats-a-comprehensive-forensic-analysis-of-the-usual-suspects.html
- Vendor/industry threat blogs: Sophos, Microsoft Security Blog, Red Canary, Huntress, Proofpoint, Trend Micro, Secureworks, ReliaQuest.

