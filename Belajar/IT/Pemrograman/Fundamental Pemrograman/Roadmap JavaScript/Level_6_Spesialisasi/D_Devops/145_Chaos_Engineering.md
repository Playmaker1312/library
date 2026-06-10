# 145: Chaos Engineering

## 1) Penjelasan

Chaos Engineering adalah **disiplin eksperimen** di mana kita sengaja memasukkan failure ke sistem untuk menguji resilience (ketahanan). Tujuannya: menemukan kelemahan sebelum kelemahan itu ditemukan oleh pengguna sungguhan. Bukan "bikin kacau asal-asalan", tapi eksperimen terkendali.

## 2) Fungsi

- **Steady State**: Definisikan "normal" — misal: latency < 200ms, error rate < 1%.
- **Hypothesis**: "Jika Pod A mati, sistem tetap responsif karena ada 3 replica."
- **Experiment**: Matikan Pod A, ukur apakah steady state tetap terjaga.
- **Tools**: Chaos Monkey (Netflix), Litmus (CNCF), Gremlin.
- **Blast Radius**: Batasi dampak eksperimen — jangan langsung prod, mulai dari staging.

## 3) Code/Config

```yaml
# litmus-experiment.yaml
apiVersion: litmuschaos.io/v1alpha1
kind: ChaosEngine
metadata:
  name: pod-kill-engine
spec:
  appinfo:
    appns: "default"
    applabel: "app=express-api"
    appkind: "deployment"
  chaosServiceAccount: litmus-admin
  experiments:
  - name: pod-delete
    spec:
      components:
        env:
        - name: TOTAL_CHAOS_DURATION
          value: "30"
        - name: CHAOS_INTERVAL
          value: "10"
        - name: PODS_AFFECTED_PERC
          value: "33"   # Matikan 1 dari 3 Pod
```

```bash
# Simulasi manual: matikan Pod dan observasi
kubectl delete pod <pod-name> --now
# Amati apakah Service tetap responsif
while true; do
  curl -s http://express-service/metrics | grep http_requests_total
  sleep 2
done
```

**Steady State Validation via PromQL:**
```promql
# Sebelum eksperimen: catat baseline
rate(http_requests_duration_seconds_sum[5m]) / rate(http_requests_duration_seconds_count[5m])

# Selama eksperimen: pantau error rate
rate(http_requests_total{status=~"5.."}[1m])
```

## 4) Analogi Rumah (Tabel)

| Konsep Chaos Engineering | Analogi Rumah |
|--------------------------|---------------|
| **Chaos Engineering** | Latihan kebakaran |
| **Steady State** | Kondisi normal — semua penghuni di lantai 2 |
| **Hypothesis** | "Jika alarm bunyi, semua penghuni keluar lewat 3 pintu darurat dalam 2 menit" |
| **Experiment** | Sengaja bunyikan alarm, evakuasi |
| **Blast Radius** | Latihan hanya di lantai 1 dulu, bukan seluruh gedung |
| **Tools (Litmus)** | Timer, pengeras suara, lembar evaluasi |
| **Resilience** | Gedung tetap aman meskipun satu tangga kebakaran terblokir |

## 5) Use Case

- Menguji apakah auto-scaling benar berjalan saat traffic spike
- Validasi bahwa circuit breaker (misal: Polly, Resilience4j) berfungsi saat service downstream mati
- Compliance / SLA — buktikan sistem bisa handle failure sesuai SLO

## 6) Kesalahan Umum

- **Tanpa hypothesis**: Asal matikan service tanpa tujuan — tidak belajar apa-apa.
- **Blast radius terlalu besar**: Eksperimen prod langsung tanpa staging — bisa menyebabkan incident beneran.
- **Tidak rollback otomatis**: Setelah eksperimen, Pod tidak pulih — sistem tetap rusak.

## 7) Benang Merah

Dari **Observability** (144) yang menyediakan alat ukur. Chaos Engineering tidak berguna tanpa observability — kita tidak bisa validasi steady state. Selanjutnya: **GitOps** (146) untuk deployment yang teraudit.

## 8) Soal

**1. Apa tujuan utama Chaos Engineering?**
Menemukan kelemahan sistem sebelum menjadi incident nyata, dengan eksperimen failure yang terkendali.

**2. Kenapa perlu steady state hypothesis sebelum eksperimen?**
Agar kita punya tolok ukur objektif: "apakah sistem masih sehat?" — bukan tebak-tebakan.

**3. Apa risiko jika blast radius terlalu besar?**
Eksperimen bisa menyebabkan downtime beneran ke pengguna, merusak data, atau memicu incident cascade.
