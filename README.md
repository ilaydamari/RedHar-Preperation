# תוכנית הכנה אינטנסיבית לראיון טכני ב־Red Hat: Cloud Infrastructure & Monitoring (Konflux) – שישה ימים עד הראיון ביום שני בבוקר

## תקציר מנהלים
התפקיד מתואר כ־“Quality + Infrastructure” יותר מאשר “רק כתיבת קוד”: המטרה היא להקשיח פלטפורמה Cloud‑native כך שהיא “production‑ready מיום 1” באמצעות שיפור CI/CD, תשתית בדיקות, ניטור בקנה מידה, וכלים פנימיים לדיבאגינג (כולל התמודדות עם flaky tests ותקלות שאינן קשורות לשינויי קוד). citeturn8view0

ההכנה האפקטיבית ביותר לראיון של ~45 דקות היא לבנות יכולת **להסביר בקול** חשיבה מערכתית ו־debugging מתודי, ולהראות שאתה “Voice of Quality” שמבין גם פרטים (פקודות, לוגים, תקלות) וגם תמונת‑על (pipelines, GitOps, supply‑chain security, observability). citeturn8view0turn4search2turn4search9

הלו״ז למטה נותן לך:
- תוכנית יומית מדויקת מחר בבוקר (יום ג׳ 17.03.2026) עד בוקר הראיון (יום ב׳ 23.03.2026), כולל מגבלת זמינות ברביעי/חמישי אחרי 20:00 בלבד.
- תוצרי‑ביניים בכל יום (סיכומי מושגים, דפי “cheat sheet”, תרגול דיבאגינג, תרגול LeetCode Medium).
- דוגמאות פקודות (Linux / kubectl), דוגמאות Terraform HCL, דוגמאות בדיקות ב‑Go (כולל Ginkgo), תרחישי דיבאגינג עם תשובות צעד‑אחר‑צעד.
- סימולציית ראיון מתוזמנת 45 דקות ביום האחרון + תסריט שאלות/תשובות, 3 נרטיבים מוכנים לפרויקטים, ו‑10 שאלות מדויקות למראיינים.

## מיפוי דרישות התפקיד לצירי אימון ולמקורות “must‑read”
המודעות למשרה מגדירה בצורה כמעט ישירה את רשימת הנושאים שראיון “אוהב” לשאול: כתיבת בדיקות ב‑Go (עדיפות ל‑Ginkgo), ניטור כשלי בדיקות ו‑flakiness, כיסוי קוד ותיעוד, הקשחת CI/CD, כתיבת כלי דיבאגינג פנימיים, ניטור מערכות בקנה מידה, עבודה עם Linux, Kubernetes, IaC (Terraform), observability (Prometheus/Grafana), Tekton ו‑ArgoCD, Multi‑arch builds, ושימוש ביקורתי ב‑LLM (“ההפך מ‑vibe coding”). citeturn8view0

כדי לייצר “סיפור מקצועי עקבי”, תשתמש בשפה של הדומיין:
- **Konflux** הוא “software factory” Cloud‑native שממוקד ב‑software supply‑chain security, hermetic builds, SBOM, provenance, integration tests ותוצאות שמחוברות ל‑SCM. citeturn1view4turn1view0  
- Konflux נשען על Tekton להרצת pipelines בקלאסטר, ויודע להיטען מאירועי Git (PR/push) ולדווח חזרה ל‑SCM (כולל שילוב עם entity["company","GitHub","code hosting platform"] ו‑entity["company","GitLab","devops platform"]). citeturn1view0  
- במודל הארכיטקטורה של Konflux יש אובייקטים כמו Application/Component/Snapshot/IntegrationTestScenario/ReleasePlan/Release (זה “מילון המושגים” שלך לשיחה מערכתית). citeturn1view1  
- ציר “supply chain compliance”: Konflux מוסיף כברירת מחדל את Conforma כ‑integration test; Conforma בודק policy/compliance על בסיס attestation של build provenance שנחתם (in‑toto) באמצעות Tekton Chains. citeturn1view2turn13view0  
- Tekton הוא הרחבה ל‑Kubernetes שמביאה CRDs (Task/Pipeline/PipelineRun וכו’) ומריצה כל Task כ‑Pod בקלאסטר. citeturn14view3turn14view0turn14view2  
- GitOps עם Argo CD: Git הוא “source of truth” למצב הרצוי, ו‑Argo CD דואג לסנכרון עם מצב הקלאסטר. citeturn0search6  
- Observability: Prometheus (scraping, jobs/instances, PromQL, alerting rules) + Grafana (alert rules). citeturn3search21turn4search1turn0search3turn4search0turn4search10  
- Multi‑arch: Docker buildx עם `--platform` (amd64/arm64), ושלוש אסטרטגיות נפוצות (QEMU emulation / multi‑node builder / cross‑compilation); ב‑Go – ההבחנה ש‑GOOS/GOARCH הם היעד, כלומר בפועל “תמיד cross‑compiling”. citeturn3search0turn3search1  
- Go testing: `testing` package + `go test`; טכניקת table‑driven tests; ובדיקות BDD עם Ginkgo שבנוי על Go testing ומודגש במשרה. citeturn2search0turn2search21turn2search4turn8view0  
- Kubernetes debugging במתודולוגיה מסודרת (איסוף מידע, describe/logs, ניטור ולוגינג) הוא “חומר ראיון” קלאסי לתפקיד כזה. citeturn2search2turn2search5  
- Terraform refresh: הבנת state ולמה הוא קיים + best practices/style. citeturn2search11turn2search15  

### תרשים זרימה שמאחד את נקודות הראיון למודל אחד
```mermaid
flowchart LR
  Dev[Developer PR/Push] --> SCM[SCM: GitHub/GitLab]
  SCM -->|Git event triggers| Konflux[Konflux CI/CD]
  Konflux --> Tekton[Tekton Pipelines on Kubernetes]
  Tekton --> TR[TaskRuns/Pods execute tests/build]
  TR --> Artifacts[OCI Images/Artifacts + SBOM]
  Tekton --> Chains[Tekton Chains signs attestations]
  Chains --> Prov[Signed provenance (in-toto/SLSA)]
  Konflux --> Conforma[Conforma policy/compliance check]
  Conforma -->|pass| Release[Release/Promotion]
  Release --> GitOps[GitOps: Argo CD sync]
  GitOps --> Cluster[Kubernetes clusters/providers]
  Cluster --> Obs[Observability: Prometheus/Grafana]
  Obs --> Alerts[Alerts + incident triage]
```

### טבלת מיקוד: מה לתרגל כדי “לפגוע” במה שהצוות מחפש
| ציר | למה זה קריטי לתפקיד | תוצר/אינדיקציה שתהיה מוכן | מקורות מועדפים |
|---|---|---|---|
| Testing strategy + Go/Ginkgo | כתיבת בדיקות, זיהוי flakiness, סקירת קוד בדיקות, כיסוי קוד/תיעוד. citeturn8view0 | אתה מסוגל לכתוב דוגמת test (table‑driven + Ginkgo) ולהסביר “למה כך”. | citeturn2search0turn2search21turn2search4 |
| CI/CD + Tekton + Konflux | התפקיד מדבר על הקשחת pipelines ו‑Konflux בנוי סביב Tekton. citeturn8view0turn1view0turn14view3 | אתה יודע להסביר Task/Pipeline/PipelineRun, ואיך תחקור PipelineRun שנכשל. | citeturn14view3turn14view2turn1view1 |
| Supply chain security | Konflux ממוקד supply‑chain security: SBOM, provenance, signed attestations, policy checks. citeturn1view4turn1view2turn13view0 | “Elevator pitch” של 60‑90 שניות על hermetic builds, SBOM, provenance, policy. | citeturn1view4turn1view2turn13view0 |
| Observability | Nice‑to‑have שמצויין; בפועל זה core אם אתה “monitoring/anomalies”. citeturn8view0 | 6 שאילתות PromQL בסיסיות + alert rule דוגמה + playbook כש־target down. | citeturn4search1turn0search3turn4search0turn3search21 |
| Kubernetes + Linux debugging | “Experience with linux systems” + “building/debugging on Kubernetes”. citeturn8view0turn2search5 | אתה עושה debug ב‑Workflow קבוע ומסביר אותו בקול. | citeturn2search2turn2search5 |
| Multi‑arch builds | ספציפית מופיע במשרה (VMs for multi‑arch builds). citeturn8view0 | אתה יודע להסביר 3 אסטרטגיות ולתת דוגמת buildx + GOARCH. | citeturn3search0turn3search1 |
| Responsible LLM use | מודגש במשרה (“ההפך מ‑vibe coding”). citeturn8view0 | יש לך פרוטוקול עבודה ברור עם LLM (verify/tests/docs). | citeturn8view0turn4search2 |

## לוח זמנים יומי מדויק מחר בבוקר עד בוקר הראיון
התאריכים לפי Asia/Jerusalem. היום: 16.03.2026 (שני בערב). הראיון: בוקר יום ב׳ 23.03.2026.  
רביעי/חמישי: זמינות רק אחרי 20:00 (נבנה בהתאם).

### סיכום חלוקת זמן כוללת (כדי שתדע “כמה זה באמת”)
| תחום | שעות נטו משוערות | למה |
|---|---:|---|
| Konflux/Tekton/GitOps/Supply Chain | 9–10 | זה הדומיין הספציפי של התפקיד. citeturn8view0turn1view1turn14view3turn0search6 |
| Testing (Go, Ginkgo, flakiness, coverage mindset) | 6–7 | זה ה־core של “Mission: Quality”. citeturn8view0turn2search4 |
| Kubernetes + Linux debugging | 6–7 | תחקור בעיות מערכת בפועל. citeturn2search5turn8view0 |
| Observability (Prometheus/Grafana/alerts) | 4–5 | “monitor anomalies” + nice‑to‑have רשמי. citeturn8view0turn4search1turn4search0 |
| Terraform/IaC refresh | 3–4 | מופיע כ‑nice‑to‑have + IaC בעבודה. citeturn8view0turn2search11 |
| LeetCode Medium | 6–7 | תואם “short technical task”. |
| Mock interview + פרויקטים + שאלות למראיינים | 4–5 | זה מה שמייצר ביצוע ב־45 דקות. |

### יום ג׳ 17.03.2026 (מחר) – בניית “שפת הדומיין”: Konflux + Tekton + GitOps, ואז 2 LeetCode
**09:00–09:30** – הקמה ותכנון (30 דק׳)  
מטרה: להגדיר “מה יוצא ממני בסוף השבוע”.  
משימה: פתח מסמך אחד בשם “Interview One‑Pager” עם 4 חלקים: (א) Elevator pitch עליך (45 שניות) (ב) Elevator pitch על Konflux (60–90 שניות) (ג) Debugging workflow קבוע (ד) 10 שאלות למראיינים (Skeleton).

**09:30–10:45** – קריאה ממוקדת: מה זה Konflux ולמה הוא קיים (75 דק׳)  
- להבין איך Konflux מציג את עצמו: CI/CD שמתמקד באבטחה והקשחה של תהליך build/test/release, כולל hermetic builds ו‑SBOM. citeturn1view4turn1view0  
תוצר: 8–10 שורות סיכום בעברית + 5 מונחים באנגלית שאתה תשתמש בהם בראיון (hermetic build, SBOM, provenance, policy, attestation).

**10:45–11:00** – הפסקה

**11:00–12:15** – קריאה ממוקדת: ארכיטקטורת Konflux והמושגים Application/Component/Snapshot/IntegrationTestScenario/Release (75 דק׳)  
- ללמוד את “הישותיות” של Konflux: Component מייצג branch/context לבנייה; Snapshot אוסף OCI artifacts בצורה אימיוטבילית; IntegrationTestScenario מייצג טסט (Tekton pipeline) שרץ על Snapshot וכו’. citeturn1view1  
תוצר: מילון מושגים (Glossary) של 10–12 מושגים + משפט אחד על כל מושג (בעברית).

**12:15–13:00** – הפסקת צהריים

**13:00–14:15** – Tekton Fundamentals (75 דק׳)  
- Tekton כ‑Kubernetes extension עם CRDs; מה ההבדל בין Task / TaskRun / Pipeline / PipelineRun. citeturn14view3  
- להבין ש‑Task רץ כ‑Pod בקלאסטר. citeturn14view0  
- להבין ש‑PipelineRun יוצר TaskRuns אוטומטית ומכיל Status שניתן לנטר. citeturn14view2  
משימה פרקטית: כתוב 3 קבצי YAML (גם אם לא תריץ אותם) – Task, Pipeline, PipelineRun (דוגמאות קצרות נמצאות בהמשך ב‑Cheat Sheets).

**14:15–14:30** – הפסקה

**14:30–15:20** – Supply Chain: Conforma + Tekton Chains (50 דק׳)  
- להבין ש‑Conforma הוא verifier/policy checker ושב‑Konflux הוא נוסף כברירת מחדל כ‑integration test; והוא בודק provenance/אינטגריטי על בסיס attestation שנחתם ע״י Tekton Chains. citeturn1view2turn13view0  
תוצר: תשובת ראיון של 90 שניות: “מה זה provenance, למה חותמים, ואיך policy check מתחבר ל‑CI”.

**15:20–16:00** – GitOps + Argo CD (40 דק׳)  
- להבין ולהסביר: Argo CD עובד לפי GitOps ושומר Git כ‑source of truth למצב הרצוי. citeturn0search6  
תוצר: 5 משפטים שאתה אומר בראיון על drift + reconciliation.

**16:00–16:15** – הפסקה

**16:15–17:15** – LeetCode Medium #1: Group Anagrams (60 דק׳) citeturn6search2  
מטרה: HashMap pattern + הסבר בקול.  
תוצר: פתרון ב‑Go + 2 דקות הסבר.

**17:15–18:15** – LeetCode Medium #2: Top K Frequent Elements (60 דק׳) citeturn6search0  
מטרה: תרגול map+heap/bucket + דיון סיבוכיות.

**18:15–18:40** – “אימון וורבאליות” (25 דק׳)  
- הקלטה עצמית (Voice note): “מה עשיתי היום ומה למדתי” (2–3 דקות).  
- ואז חזרה: “Konflux בטקסט של 90 שניות”.

### יום ד׳ 18.03.2026 – Testing ב‑Go/Ginkgo + Flaky tests (רק אחרי 20:00)
**20:00–20:15** – חימום  
- פתח את ה‑One‑Pager ותעבור על 10 המושגים של Konflux/Tekton.

**20:15–21:05** – Go testing fundamentals (50 דק׳)  
- `testing` package: טסטים הם `TestXxx(*testing.T)` ו‑`go test` מריץ פונקציות `Test*` בקבצי `_test.go`. citeturn2search0turn10search1  
- Table‑driven tests: דפוס עבודה מקובל ב‑Go. citeturn2search21  
משימה: כתוב 2 פונקציות (פשוטות) + טסט table‑driven עבורן (דוגמה בהמשך).

**21:05–21:15** – הפסקה

**21:15–22:10** – Ginkgo (55 דק׳)  
- להבין ש‑Ginkgo הוא framework “expressive specs” שיושב על Go testing, עם Gomega ל‑matchers. citeturn2search4turn2search1  
משימה: כתוב Spec אחד מינימלי + הרץ (גם בפרויקט toy).  
תוצר: אתה מסוגל להסביר למראיין מתי היית מעדיף Ginkgo (BDD, organization) ומתי “testing” vanilla.

**22:10–22:20** – הפסקה

**22:20–23:00** – Playbook Flaky Tests + LeetCode קצר  
- בניית תשובת ראיון: “איך מזהים flakiness?” (trend, correlation, retries, isolation) – זה כתוב מפורש במשרה. citeturn8view0  
- צעד פרקטי: להריץ טסטים מספר פעמים כדי לחשוף flakiness (למשל `go test -count N` כדי להריץ יותר מפעם). citeturn11search8  
- להכיר שימוש ב‑race detector בזמן בדיקות כשיש concurrency (זה נשמע מצוין בראיון, במיוחד למערכות pipelines). citeturn12search0turn12search2  
**LeetCode Medium (30–35 דק׳):** Longest Substring Without Repeating Characters citeturn7search0  
אם אתה גמור מעבודה: במקום לפתור מלא, תעשה רק “walkthrough” + כתיבת skeleton.

### יום ה׳ 19.03.2026 – Observability: Prometheus/Grafana + תחקור (רק אחרי 20:00)
**20:00–20:10** – חימום  
- 5 דקות להסביר לעצמך: “logs vs metrics vs traces” (תכין ניסוח). אפשר להישען על מושגי traces/קורלציה בעולמות telemetry. citeturn4search15turn4search22  

**20:10–21:10** – Prometheus Foundations (60 דק׳)  
- Jobs/instances: label `job` ו‑`instance` מצורפים אוטומטית בזמן scrape. citeturn3search21  
- PromQL basics (instant/range query). citeturn4search1  
משימה: כתוב במחברת 6 שאילתות PromQL “אינטואיטיביות” (לדוגמה: rate, error ratio, latency p95 אם קיים היסטוגרמה).

**21:10–21:20** – הפסקה

**21:20–22:10** – Alerting mindset (50 דק׳)  
- Prometheus alerting rules: מגדירים תנאי alert על בסיס expression. citeturn0search3  
- Grafana alert rules: “מרכז” מערכת ההתראות, עם queries/expressions ותדירות הערכה. citeturn4search0turn4search10  
משימה: תכנן alert אחד “עמיד”: error rate גבוה + “for 5m” (בעיקרון) + handling של “no data”.

**22:10–22:20** – הפסקה

**22:20–23:00** – Debugging scenario + LeetCode Medium  
- תרחיש דיבאגינג קצר (תחקור target down): מה אתה בודק בצד Prometheus (targets, networking, service discovery) ומה בצד ה‑app. השתמש במודל של Node Exporter כמשל ל‑exporter. citeturn3search2turn3search7  
**LeetCode Medium:** Search in Rotated Sorted Array citeturn7search3

### יום ו׳ 20.03.2026 – Kubernetes + Linux debugging בקצב ראיון + Graph problem
**09:00–09:25** – Linux triage drill (25 דק׳)  
תרגול: “שרת איטי / זולל CPU / זולל דיסק / בעיית רשת”. (פקודות בהמשך).

**09:25–10:45** – Kubernetes debugging (80 דק׳)  
- לקרוא את מדריכי ה‑debugging של Kubernetes (איסוף מידע, ניטור, לוגים). citeturn2search2turn2search5  
- לבנות workflow קבוע: `get → describe → logs → exec/debug → events`.  
תוצר: “דף דיבאגינג” אחד עם הפקודות שאתה משתמש בהן תמיד.

**10:45–11:00** – הפסקה

**11:00–12:15** – קוברנטיס Networking essentials (75 דק׳)  
- להבין שצריך טווחים לא חופפים של IPs ל‑Pods/Services/Nodes, בתלות ב‑network plugin / apiserver / kubelet. citeturn3search9  
משימה: להכין תשובה של 2–3 דקות: “איך היית מחבר 2 קלאסטרים” (עקרונות: CIDR לא חופף, routing, policies, DNS, trust).

**12:15–13:00** – צהריים

**13:00–14:00** – תרחיש דיבאגינג: Service לא מגיע ל‑Pods (60 דק׳)  
משימה: לכתוב “צ׳ק‑ליסט” לטיפול (selectors, endpoints, readiness).

**14:00–14:15** – הפסקה

**14:15–15:35** – LeetCode Medium: Course Schedule (80 דק׳) citeturn6search3  
מטרה: תרגל “תלות בין רכיבים” (גרפים) – זה גם מתחבר לחשיבה מערכתית.

**15:35–16:00** – סיכום יומי (25 דק׳)  
- 10 דקות – להסביר בקול: “איך אני מדבג crashloop”  
- 15 דקות – לעדכן One‑Pager.

### שבת 21.03.2026 – Terraform refresh + Multi‑arch + 2 LeetCode
**09:00–10:00** – Terraform fundamentals refresh (60 דק׳)  
- להבין את תפקיד ה‑Terraform state: מיפוי משאבים אמיתיים לקונפיגורציה, מעקב metadata, ושיפור ביצועים. citeturn2search11  

**10:00–11:00** – Workflow + Style (60 דק׳)  
- Terraform style guide: ניהול lifecycle/versioning/sensitive data וכו’. citeturn2search15  
תוצר: “סטנדרט עבודה” קצר: fmt/validate/plan/apply + איך שומרים state.

**11:00–11:15** – הפסקה

**11:15–12:15** – CI/CD + IaC angle (60 דק׳)  
משימה: להכין תשובה “איך IaC נכנס ל‑pipeline”: PR checks, plan in CI, policy checks, apply gated.

**12:15–13:00** – צהריים

**13:00–14:10** – Multi‑arch builds (70 דק׳)  
- Docker multi‑platform: `--platform linux/amd64,linux/arm64` + שלוש אסטרטגיות (QEMU / multi‑node builder / cross‑compile). citeturn3search0  
- Go cross‑compile: GOOS/GOARCH הם יעד הבנייה. citeturn3search1  
תוצר: דקה אחת הסבר “למה build ל‑arm64 נשבר לפעמים” + מה בודקים (base image, cgo, QEMU, tests).

**14:10–14:25** – הפסקה

**14:25–15:25** – LeetCode Medium: Merge Intervals (60 דק׳) citeturn6search1  

**15:25–15:40** – הפסקה

**15:40–16:40** – LeetCode Medium: Kth Largest Element in an Array (60 דק׳) citeturn7search1  

**16:40–17:20** – הכנת שאלות למראיינים (40 דק׳)  
- לבנות 10 שאלות (יש רשימה מוכנה בסוף) ולהתאים אותן לתחומי העניין שלך.

### יום א׳ 22.03.2026 – אינטגרציה + LRU Cache + סימולציה מתוזמנת 45 דק׳
**09:00–10:30** – LeetCode Medium: LRU Cache (90 דק׳) citeturn7search2  
מטרה: תרגול O(1) thinking, data structures, ויכולת להסביר “trade‑offs”.

**10:30–10:45** – הפסקה

**10:45–11:30** – Responsible LLM protocol (45 דק׳)  
- זו נקודה שמופיעה מפורשות בתיאור המשרה (“ההפך מ‑vibe coding”). citeturn8view0  
תוצר: פרוטוקול קצר של 6 צעדים (מופיע בהמשך) שאתה אומר בביטחון בראיון.

**11:30–12:10** – הכנת 3 נרטיבים לפרויקטים (40 דק׳)  
- להשתמש בפורמט STAR ולמקם את עצמך בתור מי שמשפר איכות ובונה כלים.

**12:10–13:00** – צהריים

**13:00–13:45** – בניית “ערימת הדפים” לראיון (45 דק׳)  
- דף 1: One‑Pager  
- דף 2: kubectl + Linux cheat sheet  
- דף 3: PromQL + alerting cheat sheet  
- דף 4: Konflux/Tekton glossary

**15:00–15:45** – סימולציית ראיון מתוזמנת 45 דקות (כמו ב‑Red Hat)  
(תסריט מלא בסעיף האחרון).  
מטרה: לעמוד בזמנים, להיות וורבאלי, ולהראות דרך חשיבה.

**15:45–16:15** – Post‑mortem (30 דק׳)  
- מה גמגם? מה לא היה ברור? איזה מושג חסר? לתקן ולרשום ב‑One‑Pager.

**21:30** – כיבוי מסכים ושינה מוקדמת.

### יום ב׳ 23.03.2026 (בוקר הראיון) – warm‑up קצר בלבד
**07:00–07:20** – חימום “פה‑למוח”  
- 2 דקות: “Introduce yourself” באנגלית.  
- 3 דקות: “What is Konflux?”  
- 3 דקות: “How I debug a failing test/pipeline”  
- 3 דקות: “What is flaky test?”  
- 3 דקות: “Metrics vs logs vs traces”

**07:20–07:45** – חימום קוד קל (לא לשבור ביטחון)  
- לעבור על פתרון קודם שלך של Group Anagrams או Merge Intervals ולדקלם את השלבים.

**07:45–08:10** – לעבור על 10 השאלות למראיינים ולבחור 4 “ליבה” (לפי מה שיזרום בראיון).

**08:10–08:30** – טכני: מצלמה/מיקרופון/מים/שקט.

## סט LeetCode Medium עם קישורים ופתרונות צעד‑אחר‑צעד ב‑Go
הפרוטוקול שלך לכל בעיה (זה חלק מהכנה לראיון, לא רק האלגוריתם):
1. לחזור על הבעיה במילים שלך.  
2. לתת דוגמה קטנה ידנית.  
3. להציע brute force + למה לא.  
4. להציע פתרון אופטימלי + מבני נתונים.  
5. לכתוב קוד נקי + להסביר complexity.  
6. לסיים עם edge cases.

### רשימת 8 בעיות (מומלץ להיצמד אליהן)
| # | בעיה | תבנית | זמן יעד |
|---:|---|---|---|
| 1 | Group Anagrams citeturn6search2 | HashMap (key canonical) | 35–45 דק׳ |
| 2 | Top K Frequent Elements citeturn6search0 | HashMap + Heap/Bucket | 45–60 דק׳ |
| 3 | Longest Substring Without Repeating Characters citeturn7search0 | Sliding Window | 35–45 דק׳ |
| 4 | Search in Rotated Sorted Array citeturn7search3 | Modified Binary Search | 40–55 דק׳ |
| 5 | Course Schedule citeturn6search3 | Graph + Cycle/Topo | 55–75 דק׳ |
| 6 | Merge Intervals citeturn6search1 | Sort + Sweep | 35–50 דק׳ |
| 7 | Kth Largest Element in an Array citeturn7search1 | Min‑Heap / Quickselect | 40–60 דק׳ |
| 8 | LRU Cache citeturn7search2 | HashMap + Doubly Linked List | 60–90 דק׳ |

### פתרונות (ב‑Go) – תרגול מלא
להלן “פתרונות אימון”: כתובים בצורה שתוכל גם לשחזר וגם להסביר. (מומלץ בשבת/ראשון להדפיס/להעתיק למסמך אישי).

#### Group Anagrams – HashMap לפי מפתח “מנורמל”
שלבים:
1. לכל מילה `s`, חשב מפתח ייחודי לכל אנגרמה:
   - אופציה פשוטה: למיין את האותיות (`sort.Strings`) → O(m log m) לכל מילה.
   - אופציה יעילה: לספור 26 תווים (אם lowercase) ולהשתמש במערך counts כמפתח.  
2. `map[key] -> []string`  
3. החזר את הערכים.

Complexity:
- עם sort: O(n * m log m)
- עם counts: O(n * m)

Go (גרסת counts ל‑a–z):
```go
package main

import "strconv"
import "strings"

func groupAnagrams(strs []string) [][]string {
	m := make(map[string][]string)

	for _, s := range strs {
		var cnt [26]int
		for i := 0; i < len(s); i++ {
			cnt[s[i]-'a']++
		}
		// build stable key
		var b strings.Builder
		for i := 0; i < 26; i++ {
			b.WriteByte('#')
			b.WriteString(strconv.Itoa(cnt[i]))
		}
		key := b.String()
		m[key] = append(m[key], s)
	}

	out := make([][]string, 0, len(m))
	for _, v := range m {
		out = append(out, v)
	}
	return out
}
```

איך להסביר בראיון (30 שניות):
- “I canonicalize each word to a signature. All anagrams share the same signature, so I group them in a map.”

#### Top K Frequent Elements – HashMap + Min‑Heap בגודל k
שלבים:
1. `freq[num]++`
2. לשמור heap של זוגות (value, frequency) בגודל עד k:
   - אם heap קטן מ‑k → push  
   - אחרת אם `freq > heapMin` → pop ואז push  
3. להוציא את התוצאות מה‑heap.

Complexity: O(n log k)

Go (heap מינימלי):
```go
package main

import "container/heap"

type Item struct {
	val  int
	freq int
}
type MinHeap []Item

func (h MinHeap) Len() int            { return len(h) }
func (h MinHeap) Less(i, j int) bool  { return h[i].freq < h[j].freq }
func (h MinHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *MinHeap) Push(x any)         { *h = append(*h, x.(Item)) }
func (h *MinHeap) Pop() any           { old := *h; n := len(old); x := old[n-1]; *h = old[:n-1]; return x }

func topKFrequent(nums []int, k int) []int {
	freq := make(map[int]int)
	for _, x := range nums {
		freq[x]++
	}

	h := &MinHeap{}
	heap.Init(h)

	for v, f := range freq {
		if h.Len() < k {
			heap.Push(h, Item{val: v, freq: f})
			continue
		}
		if (*h)[0].freq < f {
			heap.Pop(h)
			heap.Push(h, Item{val: v, freq: f})
		}
	}

	out := make([]int, 0, h.Len())
	for h.Len() > 0 {
		out = append(out, heap.Pop(h).(Item).val)
	}
	return out
}
```

איך להסביר:
- “We keep only k candidates in a min‑heap, so insertion stays O(log k).”

#### Longest Substring Without Repeating Characters – Sliding Window עם last seen
שלבים:
1. החזק left=0  
2. מפה `lastIndex[char]`  
3. כשנתקלת ב‑char שכבר קיים בתוך window → קפוץ את left ל‑`max(left, lastIndex[char]+1)`  
4. עדכן maxLen.

Complexity: O(n)

Go:
```go
package main

func lengthOfLongestSubstring(s string) int {
	last := make(map[byte]int)
	left, best := 0, 0

	for right := 0; right < len(s); right++ {
		c := s[right]
		if idx, ok := last[c]; ok && idx >= left {
			left = idx + 1
		}
		last[c] = right
		if right-left+1 > best {
			best = right - left + 1
		}
	}
	return best
}
```

איך להסביר:
- “Two pointers define a window with unique chars; we jump the left pointer using last seen indices.”

#### Search in Rotated Sorted Array – Binary Search עם זיהוי “איזה חצי מסודר”
שלבים:
1. mid = (l+r)/2  
2. אם nums[mid]==target → done  
3. לזהות אם החצי השמאלי מסודר (`nums[l] <= nums[mid]`):
   - אם target בתוך הטווח המסודר → search שם
   - אחרת → search בחצי השני  
4. אחרת החצי הימני מסודר – אותו היגיון.

Complexity: O(log n)

Go:
```go
package main

func searchRotated(nums []int, target int) int {
	l, r := 0, len(nums)-1
	for l <= r {
		m := l + (r-l)/2
		if nums[m] == target {
			return m
		}
		// left half sorted
		if nums[l] <= nums[m] {
			if nums[l] <= target && target < nums[m] {
				r = m - 1
			} else {
				l = m + 1
			}
		} else { // right half sorted
			if nums[m] < target && target <= nums[r] {
				l = m + 1
			} else {
				r = m - 1
			}
		}
	}
	return -1
}
```

איך להסביר:
- “At each step, one half is sorted even after rotation; I use that to decide where the target can be.”

#### Course Schedule – Detect cycle (DFS) או Topological sort
אני ממליץ להכיר topological (Kahn) כי זה “סיפור תפעולי” טוב לתלויות.

Kahn steps:
1. Build adjacency list + indegree לכל קורס  
2. Queue של כל nodes עם indegree=0  
3. Pop, “take course”, להוריד indegree לשכנים, להכניס חדשים עם 0  
4. אם ספרת numCourses → אפשר; אחרת יש cycle

Complexity: O(V+E)

Go:
```go
package main

func canFinish(numCourses int, prerequisites [][]int) bool {
	adj := make([][]int, numCourses)
	indeg := make([]int, numCourses)

	for _, p := range prerequisites {
		a, b := p[0], p[1] // to take a you need b => edge b->a
		adj[b] = append(adj[b], a)
		indeg[a]++
	}

	q := make([]int, 0)
	for i := 0; i < numCourses; i++ {
		if indeg[i] == 0 {
			q = append(q, i)
		}
	}

	seen := 0
	for head := 0; head < len(q); head++ {
		u := q[head]
		seen++
		for _, v := range adj[u] {
			indeg[v]--
			if indeg[v] == 0 {
				q = append(q, v)
			}
		}
	}
	return seen == numCourses
}
```

איך להסביר:
- “It’s dependency management: indegree=0 are ready items; if we can’t consume all nodes, there’s a cycle.”

#### Merge Intervals – Sort ואז sweep
שלבים:
1. Sort לפי start  
2. init current=[s,e] מהאינטרוול הראשון  
3. עבור כל interval:
   - אם start <= currentEnd → merge (`currentEnd=max(...)`)
   - אחרת push current ולהתחיל חדש  
4. אל תשכח push בסוף

Complexity: O(n log n)

Go:
```go
package main

import "sort"

func merge(intervals [][]int) [][]int {
	if len(intervals) == 0 {
		return nil
	}
	sort.Slice(intervals, func(i, j int) bool {
		return intervals[i][0] < intervals[j][0]
	})

	out := make([][]int, 0, len(intervals))
	curS, curE := intervals[0][0], intervals[0][1]

	for i := 1; i < len(intervals); i++ {
		s, e := intervals[i][0], intervals[i][1]
		if s <= curE {
			if e > curE {
				curE = e
			}
		} else {
			out = append(out, []int{curS, curE})
			curS, curE = s, e
		}
	}
	out = append(out, []int{curS, curE})
	return out
}
```

#### Kth Largest Element – Min‑Heap בגודל k (הכי יציב לראיון)
שלבים:
1. עבור כל num:
   - push ל‑heap
   - אם גודל > k → pop  
2. בסוף top הוא kth largest

Complexity: O(n log k)

Go:
```go
package main

import "container/heap"

type IntMinHeap []int
func (h IntMinHeap) Len() int            { return len(h) }
func (h IntMinHeap) Less(i, j int) bool  { return h[i] < h[j] }
func (h IntMinHeap) Swap(i, j int)       { h[i], h[j] = h[j], h[i] }
func (h *IntMinHeap) Push(x any)         { *h = append(*h, x.(int)) }
func (h *IntMinHeap) Pop() any           { old := *h; n := len(old); x := old[n-1]; *h = old[:n-1]; return x }

func findKthLargest(nums []int, k int) int {
	h := &IntMinHeap{}
	heap.Init(h)
	for _, x := range nums {
		heap.Push(h, x)
		if h.Len() > k {
			heap.Pop(h)
		}
	}
	return (*h)[0]
}
```

#### LRU Cache – HashMap + Doubly Linked List
הטריק: O(1) get/put דורש:
- map: key → *Node  
- doubly linked list: MRU בקצה אחד, LRU בקצה שני  
- כל גישה (get/put) מזיזה node ל‑MRU  
- כשעוברים capacity → evict LRU

Go (שלד מינימלי):
```go
package main

type Node struct {
	key, val int
	prev, next *Node
}

type LRUCache struct {
	cap int
	m map[int]*Node
	head, tail *Node // sentinels
}

func Constructor(capacity int) LRUCache {
	h := &Node{}
	t := &Node{}
	h.next = t
	t.prev = h
	return LRUCache{
		cap: capacity,
		m: make(map[int]*Node),
		head: h,
		tail: t,
	}
}

func (c *LRUCache) remove(n *Node) {
	n.prev.next = n.next
	n.next.prev = n.prev
}

func (c *LRUCache) addToMRU(n *Node) {
	// insert before tail
	p := c.tail.prev
	p.next = n
	n.prev = p
	n.next = c.tail
	c.tail.prev = n
}

func (c *LRUCache) Get(key int) int {
	if n, ok := c.m[key]; ok {
		c.remove(n)
		c.addToMRU(n)
		return n.val
	}
	return -1
}

func (c *LRUCache) Put(key int, value int) {
	if n, ok := c.m[key]; ok {
		n.val = value
		c.remove(n)
		c.addToMRU(n)
		return
	}
	n := &Node{key: key, val: value}
	c.m[key] = n
	c.addToMRU(n)

	if len(c.m) > c.cap {
		// evict LRU: head.next
		lru := c.head.next
		c.remove(lru)
		delete(c.m, lru.key)
	}
}
```

איך להסביר:
- “Hash map gives O(1) access to nodes, doubly linked list maintains recency order; each operation is constant time.”

## Playbook דיבאגינג + Cheat Sheets + דוגמאות קוד שמתחברות למשרה
### Cheat sheet: Linux triage בסיסי (בדיוק מה שמראיינים אוהבים)
```bash
# מי זולל CPU/RAM?
top
ps aux --sort=-%cpu | head
ps aux --sort=-%mem | head
free -m

# דיסק
df -h
du -sh * | sort -h

# רשת / פורטים
ss -tulpn
lsof -i :8080

# לוגים
journalctl -u <service> --since "30 min ago"
tail -n 200 -f /var/log/<file>.log
```

### Cheat sheet: Kubernetes debugging workflow (הכי “ראיוני”)
העיקרון: איסוף → עובדות → היפותזות → אימות. Kubernetes ממליץ על איסוף מידע מובנה לדיבאגינג Pods/אפליקציות. citeturn2search2turn2search5  
```bash
# מצב כללי
kubectl get pods -A
kubectl get deploy -A
kubectl get svc -A

# Pod ספציפי
kubectl describe pod <pod> -n <ns>
kubectl logs <pod> -n <ns> --since=15m
kubectl logs <pod> -n <ns> --previous  # אם יש CrashLoop

# events (לעיתים זה ה-root cause)
kubectl get events -n <ns> --sort-by=.lastTimestamp

# אם צריך להיכנס לקונטיינר
kubectl exec -it <pod> -n <ns> -- /bin/sh

# Service debugging
kubectl describe svc <svc> -n <ns>
kubectl get endpoints <svc> -n <ns>

# אם יש readiness/liveness issues -> describe יראה
```

### Cheat sheet: Tekton troubleshooting “בשפת Tekton”
מה שחשוב להסביר: Tekton Pipelines מגדיר CRDs (Task/Pipeline/PipelineRun וכו’), ו‑PipelineRun יוצר TaskRuns אוטומטית ומחזיק Status שמאפשר ניטור. citeturn14view3turn14view2  
```bash
kubectl get pipelineruns -A
kubectl describe pipelinerun <pr> -n <ns>

kubectl get taskruns -n <ns> -l tekton.dev/pipelineRun=<pr>
kubectl describe taskrun <tr> -n <ns>

# מאחר שכל Task רץ כ-Pod:
kubectl get pods -n <ns> | grep <pr-or-tr>
kubectl logs <pod> -n <ns> -c <step-container>
```

דוגמת YAML מינימלית ל‑Task/Pipeline/PipelineRun (לתרגול קריאה/הסבר):
```yaml
apiVersion: tekton.dev/v1
kind: Task
metadata:
  name: unit-tests
spec:
  workspaces:
    - name: source
  steps:
    - name: go-test
      image: golang:1.22
      workingDir: $(workspaces.source.path)
      script: |
        go test ./... -v
```

```yaml
apiVersion: tekton.dev/v1
kind: Pipeline
metadata:
  name: build-and-test
spec:
  workspaces:
    - name: source
  tasks:
    - name: run-unit-tests
      taskRef:
        name: unit-tests
      workspaces:
        - name: source
          workspace: source
```

```yaml
apiVersion: tekton.dev/v1
kind: PipelineRun
metadata:
  name: build-and-test-run
spec:
  pipelineRef:
    name: build-and-test
  workspaces:
    - name: source
      persistentVolumeClaim:
        claimName: src-pvc
```

### Cheat sheet: Terraform refresh (כי זה “ידע קודם לרפרש” אצלך)
Terraform state הוא מנגנון לייצוג הקשר בין הקונפיגורציה למשאבים האמיתיים, ושווה להסביר את זה בביטחון. citeturn2search11  
```hcl
terraform {
  required_version = ">= 1.5.0"
  required_providers {
    aws = {
      source  = "hashicorp/aws"
      version = "~> 5.0"
    }
  }
}

provider "aws" {
  region = var.region
}

variable "region" { type = string }

resource "aws_s3_bucket" "artifacts" {
  bucket = var.bucket_name
}

variable "bucket_name" { type = string }
```

Best practices “שנשמע טוב בראיון”:
- `terraform fmt` / `validate` ב‑CI
- מודולים (reusability)
- state backend מרכזי + lock (עקרון)  
- style guide של Terraform כ‑baseline. citeturn2search15

### Cheat sheet: Go tests – idiomatic + table driven + Ginkgo
Go testing בסיסי: `TestXxx(*testing.T)` בקובץ `_test.go`. citeturn2search0turn10search1  
Table‑driven הוא דפוס עבודה מוכר בקהילה. citeturn2search21  

```go
package calc

import "testing"

func Add(a, b int) int { return a + b }

func TestAdd_TableDriven(t *testing.T) {
	cases := []struct{
		name string
		a, b int
		want int
	}{
		{"pos", 1, 2, 3},
		{"zero", 0, 5, 5},
		{"neg", -1, -2, -3},
	}

	for _, tc := range cases {
		t.Run(tc.name, func(t *testing.T) {
			got := Add(tc.a, tc.b)
			if got != tc.want {
				t.Fatalf("Add(%d,%d)=%d; want %d", tc.a, tc.b, got, tc.want)
			}
		})
	}
}
```

Ginkgo (דוגמה מינימלית; אתה לא חייב להריץ, אבל חייב “לשוחח” עליה): Ginkgo מתאר את עצמו כ‑framework ל‑Go שמאפשר specs אקספרסיביות ומבוסס על Go testing + Gomega. citeturn2search4turn2search1  
```go
package calc_test

import (
	. "github.com/onsi/ginkgo/v2"
	. "github.com/onsi/gomega"
)

var _ = Describe("Add", func() {
	It("adds two ints", func() {
		Expect(1 + 2).To(Equal(3))
	})
})
```

### תרחישי דיבאגינג (עם תשובה צעד‑אחר‑צעד) – כמו בראיון
ההנחה: בראיון רוצים לראות “שיטה”. תן תמיד סדר פעולות, ואז דוגמא פקודות.

#### תרחיש: pipeline נכשל אבל אין שינוי קוד “רלוונטי” (חשד ל‑flaky)
1. לאמת שזה flaky: האם זה נכשל intermittently על אותו commit?  
2. לאסוף עובדות: באיזה שלב? unit/integration/e2e?  
3. הסתכלות על תלות חיצונית: רשת, timeouts, rate limits, clock, randomness.  
4. “ייצוב” טסט: בידוד IO חיצוני, טסט‑data קבוע, הקטנת תלות בזמן.  
5. להריץ N פעמים כדי לשחזר; ב‑Go אפשר להריץ כמה פעמים עם `-count`. citeturn11search8  
6. אם יש concurrency: להריץ עם race detector כדי לחשוף data races. citeturn12search0turn12search2  

מה להגיד כמסר איכות:
- “I treat flakiness as a production reliability bug in the CI system.”

#### תרחיש: שירות ב‑Kubernetes מחזיר 503, אבל ה‑Pods “Running”
1. `kubectl get pods` + `kubectl describe pod` – לבדוק readiness/liveness, events. citeturn2search5  
2. לבדוק Service selectors: האם הם תואמים labels של ה‑Pods?  
3. לבדוק endpoints: אם אין endpoints → selector/ready mismatch  
4. לבדוק logs של ה‑Pods (`kubectl logs`) כולל `--previous` אם יש restart  
5. אם endpoints קיימים: לבדוק NetworkPolicy / DNS / port mismatch  
6. אם latency: מדדים/לוגים/trace (אם קיים) ואז היפותזה (DB, downstream)

#### תרחיש: Prometheus “targets down”
1. Prometheus targets: האם ה‑job רואה את ה‑instance? (חשוב להבין job/instance labels). citeturn3search21  
2. בדוק connectivity (DNS, port, NetworkPolicy).  
3. בדוק exporter (למשל Node Exporter כדוגמה – הוא פשוט HTTP metrics endpoint). citeturn3search2  
4. בדוק scrape interval / timeout.  
5. אם זה “no data”: תכנן מה alerting אמור לעשות במצב כזה (Grafana/Prometheus). citeturn0search3turn4search0  

#### תרחיש: Multi‑arch build נכשל רק ב‑arm64
1. להבין מה האסטרטגיה: emulation/QEMU או cross‑compile או native builders. citeturn3search0  
2. לוודא base image תומך arm64.  
3. אם ב‑Go: לוודא GOARCH/GOOS נכונים (הם מגדירים target). citeturn3search1  
4. לבדוק cgo: אם יש תלות ב‑lib מערכת, זה עלול לשבור cross‑compile.  
5. להוסיף stage של smoke test על ה‑image לכל platform (אפילו קצר).

#### תרחיש: “סנכרון קובץ JSON בין שרתים” (מתאים להערת המועמדים מהעבר)
זה תרחיש שמודד system thinking:
1. דרישות: consistency? latency? offline?  
2. פתרון בסיסי: checksum/version + atomic write + retries.  
3. עמידות לכשל: resume אחרי נפילה (checkpoint: “מה ה‑version האחרון שהתקבל”).  
4. ניטור: metrics להצלחה/כשל, זמן סנכרון, backlog.  
5. אבטחה: חתימה/אימות/גישה (“Supply chain mindset”).

### פרוטוקול “Responsible LLM use” (תשובה שמתחברת לדרישת המשרה)
הדרישה במשרה מנסחת: לעבוד עם LLM בצורה ביקורתית, לא “vibe coding”. citeturn8view0  
תן תשובה בסגנון הזה:
1. אני משתמש ב‑LLM ל‑brainstorm (ideas, edge cases, API reminders).  
2. אני מבקש ממנו להסביר את הפתרון ולהציע בדיקות.  
3. אני **לא** מעתיק קוד בלי להבין; אני קורא docs מקוריים כשיש ספק (Tekton/Konflux/Prometheus).  
4. אני מוסיף tests ומריץ מקומית/CI.  
5. אני עושה review עצמי: סיבוכיות, race conditions, error handling.  
6. אני מתעד החלטות קצרות (למה בחרתי גישה).

זה גם “מתלבש” על תרבות open source של שקיפות ודיון מבוסס עובדות. citeturn4search2turn4search9  

## סימולציית ראיון מלאה, 3 נרטיבי פרויקטים, ו‑10 שאלות למראיינים
### תסריט סימולציה מתוזמנת 45 דקות (להריץ ביום א׳)
**דקה 0–5: Intro + פרויקט אחד**  
שאלה: “Tell me about yourself and a project you’re proud of.”  
תשובה אידיאלית (Outline):  
- 20 שניות: מי אתה ומה הכיוון (Cloud infra, CI/CD, quality mindset)  
- 60 שניות: פרויקט 1 (impact + stack)  
- 60 שניות: trade‑offs (security, reliability, observability)  
- 20 שניות: למה זה רלוונטי ל‑Konflux infra

**דקה 5–17: Tekton/Konflux system questions**  
1. מה זה Konflux ומה הוא “מוסיף” מעבר ל‑CI רגיל? (SBOM, provenance, policy). citeturn1view4turn1view2  
2. תסביר Task/Pipeline/PipelineRun ו‑איך היית מתחקור PipelineRun שנכשל. citeturn14view3turn14view2  
3. מה זה IntegrationTestScenario ומה המשמעות של Snapshot immutable? citeturn1view1  

**דקה 17–27: Testing + flakiness**  
1. Testing pyramid (unit/integration/e2e) – איך היית מחלק? citeturn8view0  
2. איך מזהים flaky test ומה עושים? (playbook מהסעיף הקודם)  
3. למה אתה מעדיף Ginkgo או לא? (תן יתרון/חיסרון). citeturn2search4turn8view0  

**דקה 27–37: Observability + production anomalies**  
1. Metrics vs logs vs traces (דקה) citeturn4search15turn4search22  
2. תסביר job/instance ב‑Prometheus ותן דוגמת PromQL. citeturn3search21turn4search1  
3. כתוב alert rule רעיוני ל‑high error rate. citeturn0search3turn4search0  

**דקה 37–45: “קוד קצר” או “דיבאגינג קצר” + שאלות שלך**  
- או תרגיל: Group Anagrams / Merge Intervals (10 דקות הסבר, לא חייב קוד מלא). citeturn6search2turn6search1  
- או תרחיש: Service 503 / targets down. citeturn2search5turn3search2  
ואז 2–3 שאלות שלך (מהרשימה למטה).

### 3 נרטיבים לפרויקטים (מוכנים – רק להתאים לפרטים האמיתיים שלך)
הכלל: אל “תנחש” מספרים. אם אין לך מדדים – תשתמש ב‑“qualitative impact” ותהיה ישר.

#### נרטיב 1: “CI/CD שהופך איכות ל‑default”
**Situation:** היה צורך להפוך deployment ליציב ומהיר, עם בדיקות אוטומטיות ושקיפות תוצאות.  
**Task:** לבנות pipeline שמבצע build/test/release בצורה עקבית, עם איכות כ‑gate.  
**Action:**  
- בניתי stages ברורים (build → unit/integration tests → artifact → deploy).  
- הוספתי בדיקות שמזהות regressions מוקדם, וחיזקתי observability (לוגים/metrics) כדי להבין failures מהר.  
- בניתי כלי עזר קטן שמקל על reproducing של failure מקומי (למשל script למשיכת לוגים/נתונים).  
**Result:** ירידה בכמות תקלות “רק בפרודקשן”, ושיפור זמן תגובה לכשלים (להוסיף נתון אמיתי).  
**Bridge לתפקיד:** זה בדיוק “engineering confidence” ו‑hardening של CI/CD כפי שמתואר במשרה. citeturn8view0  

#### נרטיב 2: “Infrastructure as Code וסטנדרט עבודה יציב”
**Situation:** תשתית נוהלה ידנית/לא עקבית (סיכון drift, קושי לשחזור).  
**Task:** להגדיר IaC ברמת team, כולל state, review, ו‑workflow.  
**Action:**  
- הקמתי Terraform בצורה מודולרית + conventions.  
- דאגתי ל‑state כבסיס למיפוי תשתית↔קוד, והוספתי CI checks (fmt/validate/plan). citeturn2search11turn2search15  
**Result:** תשתית שחזורית, פחות טעויות אנוש, ואפשרות לבצע review לפני שינוי.  
**Bridge לתפקיד:** Konflux infra עובד על IaC + multi‑provider networks, וסטנדרט עבודה חשוב ל‑scale. citeturn8view0  

#### נרטיב 3: “ניטור ותחקור תקלות בקוברנטיס”
**Situation:** מערכת microservices בקלאסטר חוותה תקלות (latency/spikes/503).  
**Task:** לאתר root cause מהר ולהחזיר יציבות.  
**Action:**  
- עבדתי לפי playbook קבוע: events/describe/logs → בדיקת Service/Endpoints → בדיקת resource pressure. citeturn2search5  
- הוספתי או שיפרתי מדדים/דאשבורדים/alerting ב‑Prometheus/Grafana כדי לתפוס אנומליות מוקדם. citeturn4search1turn4search0turn0search3  
**Result:** ירידה בזמן זיהוי הבעיה (MTTD) ובזמן תיקון (MTTR) (להוסיף נתון).  
**Bridge לתפקיד:** זה בדיוק “Monitoring large scale applications and addressing anomalies”. citeturn8view0  

### 10 שאלות “בול לתפקיד” לשאול את המראיינים
כדי להיראות חזק, שאל שאלות שמתחברות לאובייקטים/תהליכים שהם באמת חיים בהם:

1. איך אתם מגדירים “Quality gate” ב‑Konflux בפועל—מה מונע promotion אם משהו נכשל? citeturn8view0turn1view1  
2. אילו סוגי IntegrationTestScenarios אתם מריצים הכי הרבה (unit/integration/e2e) ומה ה‑pain point העיקרי? citeturn1view1turn8view0  
3. מה הסטאק שלכם ל‑observability (Prometheus/Grafana/משהו נוסף) ומה המדדים הכי חשובים לצוות? citeturn8view0turn4search1turn4search0  
4. איך אתם מתמודדים עם flaky tests לאורך זמן – יש מדיניות (quarantine, retries, ownership)? citeturn8view0  
5. איך נראה תהליך תחקור PipelineRun ב‑Tekton אצלכם—מה הכלים/הדשבורדים שנוגעים הכי הרבה? citeturn14view2turn8view0  
6. איפה נכנס Tekton Chains וחתימות/attestations ב‑flow שלכם, ומה ה‑operational complexity שזה מוסיף? citeturn13view0turn1view2  
7. איזה policies אתם בודקים עם Conforma כברירת מחדל, ומה בדרך כלל נשבר שם? citeturn1view2  
8. מה האתגרים הכי גדולים ב‑multi‑arch builds אצלכם (תזמון, אמולציה, תשתיות build, עלויות)? citeturn8view0turn3search0  
9. איך אתם עובדים GitOps‑ית (Argo CD) – מה טווח האוטומציה ומה עדיין manual/approval‑based? citeturn0search6turn8view0  
10. בתור intern—מה נחשב “הצלחה” אחרי 30/60/90 יום? איזה משימות יהיה לי סיכוי לקחת end‑to‑end?

### Checklist קצר ל‑24 השעות האחרונות
- One‑Pager מעודכן (5 דקות לעבור עליו).  
- פתרון אחד של LeetCode שאתה יכול להסביר בלי לכתוב.  
- 2 תרחישי דיבאגינג שאתה יכול “להריץ בראש” (K8s 503, flaky test).  
- 4 שאלות למראיינים שנשמעות טבעיות.  
- ברמת אנרגיה: שינה מוקדמת, מים, ארוחה קלה לפני.

