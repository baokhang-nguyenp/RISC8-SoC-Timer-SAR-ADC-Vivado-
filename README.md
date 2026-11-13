# 5-Stage Pipeline RISC-8 SoC with APB Timer & SAR ADC

An 8-bit System-on-Chip (SoC) built around a 5-stage pipeline RISC-8 processor, integrating an 8-bit Timer (with PWM generation) and a 4-channel 8-bit SAR ADC via an APB Interconnect. Designed and verified on **Xilinx Vivado (XSim)** using SystemVerilog and **UVM (Universal Verification Methodology)**.

---

## 1. System Architecture

The SoC follows a single-master APB architecture where the 5-stage pipelined RISC-8 CPU core controls memory and peripheral endpoints:
* **CPU Core (`risc8_5stage_core`):** 5-stage pipeline processor executing instructions from Instruction Memory (`IMEM`).
* **APB Interconnect (`timer_adc8_cpu_if`):** Bridges CPU I/O requests (`io_addr`, `io_wdata`, `io_rdata`, `io_we`, `io_cs`) to memory-mapped peripheral registers.
* **8-Bit Timer & PWM (`timer8`):** Configurable prescaler (`TMR_PRESC`), period (`TMR_LOAD`), and threshold (`TMR_CMP`). Generates single-cycle overflow (`ovf_pulse`), compare match (`cmp_pulse`), and continuous PWM output (`pwm_out = (cnt < cmp)`).
* **8-Bit SAR ADC (`sar_adc_ctrl`):** 4-channel analog multiplexer (`ch0`–`ch3`), Sample & Hold control, and an 8-step binary search engine ($V_{\text{ref}} = 3.3\text{V}$, $\text{LSB} \approx 12.89\text{mV}$). Triggers an End-of-Conversion (`EOC`) interrupt upon completion.


---

## 2. Directory Structure

```text
├── rtl/          # Verilog RTL design sources (CPU, APB wrapper, Timer, SAR ADC)
├── sim/          # System-level testbench (tb_risc8_soc.sv) & UVM verification environments
├── sw/           # Compiled machine code (soc_demo.mem) for IMEM initialization
└── docs/         # Presentation slides and simulation waveform screenshots
