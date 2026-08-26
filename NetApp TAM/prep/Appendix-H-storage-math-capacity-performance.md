# Appendix H - Storage Math, Capacity, Performance, and Forecasting Workbook Guide

> **Purpose:** Turn storage quantities into defensible calculations for interviews, service reviews, capacity/performance analysis, replication planning, recovery objectives, availability, and scenario decisions.
>
> **How to use:** Write assumptions and units first, calculate, perform a unit check, reproduce the result in Excel, interpret the business meaning, then state the trap and uncertainty.
>
> **Reference date:** 2026-08-24

## Scope, safety, and evidence boundaries

- All values are synthetic and use `SYN-` scenarios. They are arithmetic exercises, not sizing guidance, guarantees, platform limits, SLAs, or customer recommendations.
- RAID examples are generic simplifications. Real ONTAP/platform usable capacity depends on supported RAID type, layout, spares, partitioning, WAFL/system reserves, drive sizing, metadata, release, and configuration rules. Verify current HWU, IMT, product docs, and sizing tools.
- Efficiency, snapshot, tiering, performance, availability, and forecast outcomes are workload- and definition-dependent. Never promise a ratio or extrapolate a benchmark without equivalent conditions.
- Excel formulas use English function names and comma separators; locale/version behavior can differ. Preserve full precision in calculations and round only for display.
- Protect customer inventory and telemetry. Record owner, source, cutoff date, units, confidence, validation, and residual risk.
- See [Parts 4-10](Part-04-data-storage-bits-blocks-files-objects.md), [Parts 43-46](Part-43-ontap-performance-counters.md), and [Part 59](Part-59-excel-tam-analysis.md).

## Calculation contract

| Field | Required |
|---|---|
| Scenario ID | `SYN-MATH-<number>` |
| Inputs | Values, units, source/cutoff, scope |
| Assumptions | Model, exclusions, time basis, rounding |
| Formula | Symbolic and substituted form |
| Unit check | Cancellation and output unit |
| Excel | Equivalent formula using cells or constants |
| Interpretation | Decision meaning, not only the number |
| Trap | Misuse or hidden dependency |
| Governance | Owner/source/date/confidence/validation/residual risk |

```mermaid
flowchart LR
    H00I[Inputs with units] --> H00A[Assumptions and scope]
    H00A --> H00F[Formula and substitution]
    H00F --> H00U[Unit and reasonableness check]
    H00U --> H00E[Excel reproduction]
    H00E --> H00S[Scenario interpretation]
    H00S --> H00D[Decision confidence and residual risk]
```

## 1. Decimal and binary conversions

### Worked H01 - Decimal terabytes to bytes

**Assumptions:** `SYN-MATH-01` uses SI decimal units; $1\ \mathrm{TB}=10^{12}\ \mathrm{bytes}$.

$$10\ \mathrm{TB} * 10^{12}\ \frac{\mathrm{bytes}}{\mathrm{TB}} = 10{,}000{,}000{,}000{,}000\ \mathrm{bytes}$$

**Unit check:** TB cancels, leaving bytes. **Excel:** `=10*10^12`. **Interpretation:** A marketed 10 TB value is $10^{13}$ bytes. **Trap:** Do not call this 10 TiB. **Governance:** owner `<analyst>`; source `synthetic`; date `2026-08-24`; confidence high; validate calculator/Excel; residual risk none for arithmetic.

### Worked H02 - Decimal terabytes to tebibytes

**Assumptions:** `SYN-MATH-02`; $1\ \mathrm{TiB}=2^{40}$ bytes.

$$10\ \mathrm{TB} * \frac{10^{12}\ \mathrm{bytes}}{1\ \mathrm{TB}} * \frac{1\ \mathrm{TiB}}{2^{40}\ \mathrm{bytes}} = 9.094947\ \mathrm{TiB}$$

**Unit check:** TB and bytes cancel. **Excel:** `=10*10^12/2^40`. **Interpretation:** The same bytes display as about 9.095 TiB. **Trap:** The difference is units, not missing capacity.

### Worked H03 - Tebibytes to decimal terabytes

**Assumptions:** `SYN-MATH-03`; convert 8 TiB without overhead.

$$8\ \mathrm{TiB} * \frac{2^{40}\ \mathrm{bytes}}{1\ \mathrm{TiB}} * \frac{1\ \mathrm{TB}}{10^{12}\ \mathrm{bytes}} = 8.796093\ \mathrm{TB}$$

**Unit check:** TiB and bytes cancel. **Excel:** `=8*2^40/10^12`. **Interpretation:** 8 TiB contains about 8.796 decimal TB. **Trap:** Never compare displayed numbers without unit labels.

### Worked H04 - Link bits per second to ideal bytes per second

**Assumptions:** `SYN-MATH-04`; ideal 10 Gb/s line rate, ignoring framing/protocol overhead.

$$10\ \mathrm{Gb/s} * \frac{1\ \mathrm{GB}}{8\ \mathrm{Gb}} = 1.25\ \mathrm{GB/s}$$

**Unit check:** Gb cancels, leaving GB/s. **Excel:** `=10/8`. **Interpretation:** The theoretical byte rate is 1.25 GB/s before overhead. **Trap:** Do not promise payload throughput from line rate.

## 2. Raw, usable, RAID, and efficiency

### Worked H05 - Raw drive capacity

**Assumptions:** `SYN-MATH-05`; twelve equal 8 TB decimal drives, no protection/overhead.

$$12\ \mathrm{drives} * 8\ \frac{\mathrm{TB}}{\mathrm{drive}} = 96\ \mathrm{TB\ raw}$$

**Unit check:** drives cancel. **Excel:** `=12*8`. **Interpretation:** Raw nominal capacity is 96 TB. **Trap:** Raw is not usable or application-available.

### Worked H06 - Generic single-parity planning estimate

**Assumptions:** `SYN-MATH-06`; twelve 8 TB drives, one equivalent drive of parity, no spares/system overhead; generic only.

$$C_{usable}=(12-1)*8\ \mathrm{TB}=88\ \mathrm{TB}$$

**Unit check:** drive count times TB/drive gives TB. **Excel:** `=(12-1)*8`. **Interpretation:** Simplified pre-overhead estimate is 88 TB. **Trap:** This is not an ONTAP layout rule or sizing output.

### Worked H07 - Generic dual-parity planning estimate

**Assumptions:** `SYN-MATH-07`; twelve 8 TB drives, two equivalent parity drives, no spare/system overhead.

$$C_{usable}=(12-2)*8\ \mathrm{TB}=80\ \mathrm{TB}$$

**Unit check:** count times capacity yields TB. **Excel:** `=(12-2)*8`. **Interpretation:** Simplified pre-overhead estimate is 80 TB. **Trap:** Rebuild risk, RAID-group shape, spares, and platform rules are omitted.

### Worked H08 - Mirrored capacity estimate

**Assumptions:** `SYN-MATH-08`; ten 3.84 TB devices in equal two-way mirrors, no overhead.

$$C_{usable}=\frac{10*3.84\ \mathrm{TB}}{2}=19.2\ \mathrm{TB}$$

**Unit check:** device count times TB/device, divided by copies, yields TB. **Excel:** `=10*3.84/2`. **Interpretation:** Half the raw capacity remains before overhead. **Trap:** Mirroring does not replace backup.

### Worked H09 - Usable percentage after generic overhead

**Assumptions:** `SYN-MATH-09`; 80 TB protected capacity becomes 70 TB usable after all stated synthetic overhead.

$$\mathrm{Usable\%}=\frac{70}{80}*100=87.5\%$$

**Unit check:** TB/TB cancels to a ratio. **Excel:** `=70/80`. **Interpretation:** 12.5% of the protected pool is unavailable to this usable scope. **Trap:** Explain exactly which overheads are included.

### Worked H10 - Data-efficiency ratio and savings

**Assumptions:** `SYN-MATH-10`; 60 TiB logical data occupies 30 TiB physical in the same scope/time.

$$\mathrm{Ratio}=\frac{60}{30}=2.0:1,\qquad \mathrm{Savings}=1-\frac{30}{60}=50\%$$

**Unit check:** TiB/TiB cancels. **Excel:** ratio `=60/30`; savings `=1-30/60`. **Interpretation:** Each physical TiB represents two logical TiB for this dataset. **Trap:** Do not apply the ratio to a different workload or mix logical/physical scopes.

## 3. Utilization, headroom, thin provisioning, snapshots, and tiering

### Worked H11 - Utilization and headroom to an operating threshold

**Assumptions:** `SYN-MATH-11`; 52 TB used of 80 TB usable; planning threshold 80% is a policy assumption.

$$U=\frac{52}{80}=65\%,\qquad H=(0.80*80)-52=12\ \mathrm{TB}$$

**Unit check:** utilization is dimensionless; threshold capacity minus used yields TB. **Excel:** `=52/80`; `=80%*80-52`. **Interpretation:** 12 TB remains before the synthetic 80% threshold. **Trap:** Threshold is not a NetApp universal hard limit.

### Worked H12 - Thin-provisioning overcommit

**Assumptions:** `SYN-MATH-12`; 150 TB provisioned against 100 TB usable; 75 TB physically used.

$$\mathrm{Overcommit}=\frac{150}{100}=150\%,\qquad \mathrm{Free}=100-75=25\ \mathrm{TB}$$

**Unit check:** overcommit is a ratio; free is TB. **Excel:** `=150/100`; `=100-75`. **Interpretation:** Logical promises exceed backing by 50 TB while 25 TB remains physically free now. **Trap:** Provisioned is not consumed, and current free space is not future safety.

### Worked H13 - Snapshot unique-block consumption

**Assumptions:** `SYN-MATH-13`; active data consumes 40 TiB and snapshot-only unique blocks consume 12 TiB; other overhead excluded.

$$C_{physical}=40+12=52\ \mathrm{TiB},\qquad \mathrm{Snapshot\ ratio}=\frac{12}{40}=30\%$$

**Unit check:** TiB + TiB = TiB; ratio cancels. **Excel:** `=40+12`; `=12/40`. **Interpretation:** Snapshot-only blocks add 30% relative to active physical data. **Trap:** Snapshot behavior depends on change rate and retention, not snapshot count alone.

### Worked H14 - Simple tiering split

**Assumptions:** `SYN-MATH-14`; 80 TiB physical eligible data, 25% cold and tiered; efficiency and metadata ignored.

$$C_{cold}=80*25\%=20\ \mathrm{TiB},\qquad C_{performance}=80-20=60\ \mathrm{TiB}$$

**Unit check:** TiB times ratio yields TiB. **Excel:** `=80*25%`; `=80-80*25%`. **Interpretation:** Synthetic split leaves 60 TiB on the performance tier. **Trap:** Eligibility, cooling age, recalls, and cloud cost are omitted.

## 4. Growth, CAGR, linear, exponential, seasonal, and scenarios

### Worked H15 - Absolute monthly growth

**Assumptions:** `SYN-MATH-15`; used capacity rises from 42 to 48 TiB over three equal months.

$$g=\frac{48-42}{3}=2\ \mathrm{TiB/month}$$

**Unit check:** TiB/month. **Excel:** `=(48-42)/3`. **Interpretation:** Average absolute growth is 2 TiB/month. **Trap:** Three points can hide seasonality and one-time loads.

### Worked H16 - Percentage growth over a period

**Assumptions:** `SYN-MATH-16`; same 42 to 48 TiB period.

$$g_{pct}=\frac{48-42}{42}*100=14.2857\%$$

**Unit check:** TiB/TiB cancels. **Excel:** `=(48-42)/42`. **Interpretation:** Capacity increased about 14.29% from the starting value. **Trap:** This is total-period growth, not monthly growth.

### Worked H17 - CAGR

**Assumptions:** `SYN-MATH-17`; capacity grows from 100 to 133.1 TiB over three years.

$$\mathrm{CAGR}=\left(\frac{133.1}{100}\right)^{1/3}-1=0.10=10\%$$

**Unit check:** ratio is dimensionless; exponent is per year. **Excel:** `=(133.1/100)^(1/3)-1`. **Interpretation:** A constant 10% annual compound rate connects endpoints. **Trap:** CAGR hides every intervening rise and fall.

### Worked H18 - Linear forecast

**Assumptions:** `SYN-MATH-18`; current 60 TiB, constant 2 TiB/month for six months.

$$C_6=60+(2*6)=72\ \mathrm{TiB}$$

**Unit check:** TiB + TiB/month * month = TiB. **Excel:** `=60+2*6`. **Interpretation:** Linear projection reaches 72 TiB. **Trap:** Constant absolute growth is an assumption, not a discovered law.

### Worked H19 - Exponential forecast

**Assumptions:** `SYN-MATH-19`; current 60 TiB grows 4% monthly for six months.

$$C_6=60*(1+0.04)^6=75.9191\ \mathrm{TiB}$$

**Unit check:** TiB times a dimensionless factor. **Excel:** `=60*(1+4%)^6`. **Interpretation:** Compounding adds about 15.92 TiB. **Trap:** Exponential growth can overstate long horizons when demand saturates.

### Worked H20 - Seasonal peak forecast

**Assumptions:** `SYN-MATH-20`; 70 TiB now, trend 1.5 TiB/month for four months, known seasonal peak adds 12 TiB.

$$C_{peak}=70+(1.5*4)+12=88\ \mathrm{TiB}$$

**Unit check:** all terms are TiB. **Excel:** `=70+1.5*4+12`. **Interpretation:** Peak scenario is 88 TiB. **Trap:** Do not count a seasonal effect twice if historical trend already includes it.

### Worked H21 - Low, base, and high scenarios

**Assumptions:** `SYN-MATH-21`; 50 TiB current, six months; rates 1, 1.8, and 3 TiB/month.

$$C_{low}=56,\qquad C_{base}=60.8,\qquad C_{high}=68\ \mathrm{TiB}$$

**Unit check:** current TiB + rate * months. **Excel:** `=50+1*6`, `=50+1.8*6`, `=50+3*6`. **Interpretation:** Decision range is 56-68 TiB. **Trap:** Scenarios need evidence and triggers, not arbitrary optimism/pessimism.

### Worked H22 - Low-base percentage trap

**Assumptions:** `SYN-MATH-22`; a small workload grows from 1 to 2 TiB.

$$\mathrm{Growth}=\frac{2-1}{1}*100=100\%,\qquad \mathrm{Absolute\ change}=1\ \mathrm{TiB}$$

**Unit check:** percentage is dimensionless; change is TiB. **Excel:** `=(2-1)/1`; `=2-1`. **Interpretation:** Growth doubled but added only 1 TiB. **Trap:** Percentage alone exaggerates low-base materiality.

## 5. Time to threshold and latest safe start

### Worked H23 - Time to threshold

**Assumptions:** `SYN-MATH-23`; 58 TiB used, threshold 80 TiB, linear growth 2.2 TiB/month.

$$t=\frac{80-58}{2.2}=10\ \mathrm{months}$$

**Unit check:** TiB divided by TiB/month = months. **Excel:** `=(80-58)/2.2`. **Interpretation:** Threshold is ten months away under the model. **Trap:** Growth may accelerate and threshold may be policy-based.

### Worked H24 - Latest safe action start

**Assumptions:** `SYN-MATH-24`; threshold in 10 months, implementation lead time 4 months, contingency buffer 1 month.

$$t_{start}=10-4-1=5\ \mathrm{months\ from\ now}$$

**Unit check:** all terms are months. **Excel:** `=10-4-1`. **Interpretation:** Start by month five to preserve the buffer. **Trap:** Procurement/change freezes and approvals may increase lead time.

### Worked H25 - New workload impact on threshold

**Assumptions:** `SYN-MATH-25`; 62 TiB used, 80 TiB threshold, immediate 8 TiB onboarding, then 1.5 TiB/month.

$$C_{after}=62+8=70\ \mathrm{TiB},\qquad t=\frac{80-70}{1.5}=6.6667\ \mathrm{months}$$

**Unit check:** TiB then TiB/(TiB/month) = months. **Excel:** `=62+8`; `=(80-(62+8))/1.5`. **Interpretation:** Onboarding reduces runway to about 6.7 months. **Trap:** Include snapshots, protection copies, and ramp profile separately.

### Worked H26 - Threshold date from monthly runway

**Assumptions:** `SYN-MATH-26`; forecast starts 2026-09-01 and runway is 6.67 months; use a conservative seven-month calendar check.

$$\mathrm{Review\ date}=\mathrm{EDATE}(\text{2026-09-01},7)=\text{2027-04-01}$$

**Unit check:** date + months = date. **Excel:** `=EDATE(DATE(2026,9,1),7)`. **Interpretation:** April 1 is a conservative calendar review marker, not the exact crossing instant. **Trap:** Month length and fractional-month conventions require explicit handling.

## 6. IOPS, throughput, I/O size, latency, queues, and percentiles

### Worked H27 - IOPS from throughput and I/O size

**Assumptions:** `SYN-MATH-27`; 800 MiB/s, average 64 KiB per operation, same binary units.

$$\mathrm{IOPS}=800\ \frac{\mathrm{MiB}}{\mathrm{s}}*\frac{1024\ \mathrm{KiB}}{1\ \mathrm{MiB}}*\frac{1\ \mathrm{I/O}}{64\ \mathrm{KiB}}=12{,}800\ \mathrm{IOPS}$$

**Unit check:** MiB and KiB cancel, leaving I/O/s. **Excel:** `=800*1024/64`. **Interpretation:** 800 MiB/s at 64 KiB averages 12,800 IOPS. **Trap:** Mixed sizes need operation-weighted distribution, not one guessed size.

### Worked H28 - Throughput from IOPS and I/O size

**Assumptions:** `SYN-MATH-28`; 25,000 IOPS at 16 KiB average.

$$T=25{,}000\ \frac{\mathrm{I/O}}{\mathrm{s}}*16\ \frac{\mathrm{KiB}}{\mathrm{I/O}}*\frac{1\ \mathrm{MiB}}{1024\ \mathrm{KiB}}=390.625\ \mathrm{MiB/s}$$

**Unit check:** I/O and KiB cancel appropriately. **Excel:** `=25000*16/1024`. **Interpretation:** Payload estimate is 390.625 MiB/s. **Trap:** Protocol overhead and retries make wire/storage rates differ.

### Worked H29 - Mixed operation-weighted I/O size

**Assumptions:** `SYN-MATH-29`; 70% of operations are 8 KiB and 30% are 64 KiB; 20,000 total IOPS.

$$S_{avg}=0.7*8+0.3*64=24.8\ \mathrm{KiB},\qquad T=20{,}000*24.8/1024=484.375\ \mathrm{MiB/s}$$

**Unit check:** weighted size is KiB/I/O; multiplying by I/O/s yields KiB/s. **Excel:** `=(70%*8+30%*64)*20000/1024`. **Interpretation:** Operation-weighted average gives about 484.4 MiB/s. **Trap:** A byte-weighted mix would answer a different question.

### Worked H30 - End-to-end latency composition

**Assumptions:** `SYN-MATH-30`; synchronized synthetic component times: host 0.4 ms, network 0.8 ms, storage service 2.6 ms, return/application 0.7 ms.

$$W=0.4+0.8+2.6+0.7=4.5\ \mathrm{ms}$$

**Unit check:** ms + ms = ms. **Excel:** `=SUM(0.4,0.8,2.6,0.7)`. **Interpretation:** Synthetic total is 4.5 ms, with storage service the largest component. **Trap:** Unsynchronized counters and overlapping work may not add directly.

### Worked H31 - Little's Law outstanding I/O

**Assumptions:** `SYN-MATH-31`; stable average rate 18,000 IOPS and average response time 6 ms = 0.006 s.

$$L=\lambda W=18{,}000\ \frac{\mathrm{I/O}}{\mathrm{s}}*0.006\ \mathrm{s}=108\ \mathrm{I/O}$$

**Unit check:** seconds cancel. **Excel:** `=18000*(6/1000)`. **Interpretation:** About 108 I/Os are outstanding on average in the defined system. **Trap:** Little's Law needs consistent scope and a stable observation interval.

### Worked H32 - Queue growth when latency rises

**Assumptions:** `SYN-MATH-32`; rate stays 20,000 IOPS; latency rises from 3 to 9 ms.

$$L_{before}=20{,}000*0.003=60,\qquad L_{after}=20{,}000*0.009=180$$

**Unit check:** I/O/s * s = I/O. **Excel:** `=20000*3/1000`; `=20000*9/1000`. **Interpretation:** Average outstanding work triples. **Trap:** Queue growth can be consequence or cause; this equation alone does not locate the bottleneck.

### Worked H33 - Nearest-rank p90

**Assumptions:** `SYN-MATH-33`; sorted latencies in ms are 2,2,3,3,4,4,5,8,12,20; nearest-rank method.

$$r=\lceil0.90*10\rceil=9,\qquad p90=x_9=12\ \mathrm{ms}$$

**Unit check:** rank is dimensionless; selected value is ms. **Excel:** `=PERCENTILE.INC(A1:A10,0.9)` may interpolate and differ. **Interpretation:** Nearest-rank p90 is 12 ms. **Trap:** State percentile method and sample size.

### Worked H34 - Average hides tail latency

**Assumptions:** `SYN-MATH-34`; 980 requests at 2 ms and 20 requests at 100 ms.

$$\bar{x}=\frac{980*2+20*100}{1000}=3.96\ \mathrm{ms},\qquad p99=100\ \mathrm{ms}$$

**Unit check:** total ms observations / count = ms. **Excel:** average `=(980*2+20*100)/1000`. **Interpretation:** Average looks low while the slowest 2% make p99 100 ms. **Trap:** Do not report only averages for user experience.

## 7. Replication bandwidth, RPO, and RTO

### Worked H35 - Initial replication bandwidth

**Assumptions:** `SYN-MATH-35`; transfer 2 TiB in 8 hours, continuous ideal payload, then add 20% planning overhead.

$$T=\frac{2*1024*1024\ \mathrm{MiB}}{8*3600\ \mathrm{s}}=72.8178\ \mathrm{MiB/s}$$

$$B=72.8178*8*1.20=699.0507\ \mathrm{Mib/s}$$

**Unit check:** MiB/s times 8 = Mib/s; overhead is dimensionless. **Excel:** `=2*1024*1024/(8*3600)*8*1.2`. **Interpretation:** Plan about 699 Mib/s under these simplified assumptions. **Trap:** Compression, dedupe, TCP efficiency, contention, throttles, and change during baseline are omitted.

### Worked H36 - Sustained change-rate bandwidth

**Assumptions:** `SYN-MATH-36`; 120 GiB changes each hour; 25% overhead.

$$T=\frac{120*1024}{3600}=34.1333\ \mathrm{MiB/s},\qquad B=34.1333*8*1.25=341.3333\ \mathrm{Mib/s}$$

**Unit check:** GiB/hour converts to MiB/s then Mib/s. **Excel:** `=120*1024/3600*8*1.25`. **Interpretation:** About 341.3 Mib/s is needed to keep pace in this model. **Trap:** Bursty change requires headroom beyond the average.

### Worked H37 - Observed recovery-point gap

**Assumptions:** `SYN-MATH-37`; last validated recovery point 10:00 UTC; incident 10:23 UTC; target RPO 15 minutes.

$$\mathrm{Gap}=23\ \mathrm{min},\qquad \mathrm{Target\ variance}=23-15=8\ \mathrm{min}$$

**Unit check:** minutes minus minutes. **Excel:** `=(TIME(10,23,0)-TIME(10,0,0))*1440`; variance `=23-15`. **Interpretation:** Observed potential data-loss window exceeds target by 8 minutes. **Trap:** A timestamped copy is not automatically application-consistent or recoverable.

### Worked H38 - RTO component budget

**Assumptions:** `SYN-MATH-38`; detection 8 min, decision 12, restore 45, validation 20.

$$RTO_{actual}=8+12+45+20=85\ \mathrm{minutes}$$

**Unit check:** minutes. **Excel:** `=SUM(8,12,45,20)`. **Interpretation:** End-to-end recovery takes 85 minutes; restore execution is only one component. **Trap:** Do not equate copy availability with service recovery.

## 8. Availability and downtime

### Worked H39 - Monthly downtime at 99.9%

**Assumptions:** `SYN-MATH-39`; 30-day month, continuous service, availability definition excludes no periods.

$$D=(1-0.999)*30*24*60=43.2\ \mathrm{minutes}$$

**Unit check:** dimensionless unavailability * days * hours/day * minutes/hour. **Excel:** `=(1-99.9%)*30*24*60`. **Interpretation:** 99.9% permits 43.2 minutes in this synthetic month. **Trap:** SLA windows, exclusions, rounding, and measurement source change the result.

### Worked H40 - Annual downtime at 99.99%

**Assumptions:** `SYN-MATH-40`; 365-day year.

$$D=(1-0.9999)*365*24*60=52.56\ \mathrm{minutes}$$

**Unit check:** ratio times minutes/year = minutes. **Excel:** `=(1-99.99%)*365*24*60`. **Interpretation:** Four nines corresponds to about 52.56 minutes/year. **Trap:** Availability is not durability or performance.

### Worked H41 - Serial dependency availability

**Assumptions:** `SYN-MATH-41`; two required independent components have 99.99% and 99.9% availability; both must work.

$$A_{series}=0.9999*0.999=0.9989001=99.89001\%$$

**Unit check:** ratios multiply. **Excel:** `=99.99%*99.9%`. **Interpretation:** The series service is less available than either component. **Trap:** Independence and identical measurement windows may be false.

### Worked H42 - Parallel redundancy estimate

**Assumptions:** `SYN-MATH-42`; two truly independent equivalent components, each 99% available, either can serve full load and failover succeeds.

$$A_{parallel}=1-(1-0.99)^2=0.9999=99.99\%$$

**Unit check:** probability is dimensionless. **Excel:** `=1-(1-99%)^2`. **Interpretation:** Ideal independent redundancy raises estimated availability. **Trap:** Shared power/network/software and failed failover invalidate the independence model.

## 9. Confidence and sensitivity

### Worked H43 - Approximate confidence interval for a mean

**Assumptions:** `SYN-MATH-43`; 36 approximately independent observations, mean 50 ms, sample standard deviation 6 ms; rough 95% multiplier 2 for teaching.

$$SE=\frac{6}{\sqrt{36}}=1\ \mathrm{ms},\qquad CI\approx50\mathbin{\pm}2*1=[48,52]\ \mathrm{ms}$$

**Unit check:** standard error and interval remain ms. **Excel:** lower `=50-2*(6/SQRT(36))`; upper `=50+2*(6/SQRT(36))`. **Interpretation:** Under assumptions, mean uncertainty is roughly 48-52 ms. **Trap:** Use the correct distribution, dependence treatment, and sampling design in real analysis.

### Worked H44 - Forecast sensitivity to growth rate

**Assumptions:** `SYN-MATH-44`; 60 TiB used, threshold 80 TiB; low/base/high growth 1.5/2/2.5 TiB/month.

$$t_{low}=\frac{20}{1.5}=13.33,\qquad t_{base}=\frac{20}{2}=10,\qquad t_{high}=\frac{20}{2.5}=8\ \mathrm{months}$$

**Unit check:** TiB divided by TiB/month = months. **Excel:** `=(80-60)/1.5`, `=(80-60)/2`, `=(80-60)/2.5`. **Interpretation:** Plausible rate variation moves the threshold by over five months, so action timing should follow the high case plus lead-time buffer. **Trap:** A single forecast date hides decision sensitivity.

## Workbook layout

| Sheet/table | Purpose | Minimum fields |
|---|---|---|
| `Inputs` | Controlled synthetic/customer inputs | value, unit, source, cutoff, owner, confidence |
| `Conversions` | Decimal/binary and rate normalization | original, formula, normalized, unit check |
| `Capacity` | Raw/usable/used/logical/physical/headroom | object, scope, definitions, snapshot/tiering/efficiency |
| `Growth` | Time series and scenarios | date, value, event, low/base/high, model error |
| `Performance` | IOPS/throughput/size/latency/queue | interval, object, workload, percentile, source |
| `Protection` | Transfer/RPO/RTO calculations | change, bandwidth, overhead, copy/restore/validate times |
| `Availability` | Service window and dependency math | component/service, interval, exclusions, downtime |
| `QA` | Reconciliation and formula tests | check, expected, actual, pass/fail, owner |
| `Output` | Decision-ready findings | evidence, risk, action, owner, date, validation, residual risk |

## Math QA checklist

- [ ] Decimal/binary and bit/byte conventions are explicit.
- [ ] Numerator, denominator, object scope, interval, timezone, and exclusions match.
- [ ] Percentages are stored as decimals and formatted, not multiplied twice.
- [ ] Rates use compatible time units; latency milliseconds are converted to seconds for Little's Law.
- [ ] RAID math is labeled generic and cross-checked with current authoritative sizing sources.
- [ ] Logical, physical, raw, usable, effective, provisioned, and used are not mixed.
- [ ] Snapshot/tiering/efficiency scopes are non-overlapping or reconciliation is explicit.
- [ ] Forecast includes seasonality/events, low/base/high scenarios, lead time, and model error.
- [ ] Percentile method, sample size, and aggregation interval are recorded.
- [ ] Replication includes change during transfer, overhead, contention, and validation constraints.
- [ ] Availability definition, window, exclusions, dependencies, and independence assumptions are recorded.
- [ ] Excel formulas are peer-checked against hand calculations and synthetic edge cases.
- [ ] Owner/source/date/confidence/validation/residual risk accompany every published result.

## Completion and use checklist

- [x] 44 numbered, fully worked synthetic calculations exceed the required 40.
- [x] Every worked example includes assumptions, KaTeX, unit check, Excel formula, interpretation, and trap.
- [x] Decimal/binary, raw/usable, generic RAID, efficiency, utilization/headroom, thin/overcommit, snapshot/tiering, growth/CAGR, linear/exponential/seasonal scenarios, threshold timing, performance/queues/percentiles, replication/RPO/RTO, availability, confidence, and sensitivity are covered.
- [x] No hard limit, guaranteed efficiency/performance, or product-specific sizing rule is invented.
- [ ] Before customer use, replace synthetic inputs with authorized evidence and run the QA checklist.

---

*Navigation:* Previous: [Appendix G - Troubleshooting and Major-Incident Field Manual](Appendix-G-troubleshooting-incident-field-manual.md) | Next: [Appendix I - Excel, Power BI, and PowerPoint TAM Toolkit](Appendix-I-office-tam-toolkit.md) | [Master guide](../NetApp%20TAM%20Technical%20Analyst%20-%20Complete%20Study%20Guide.md)