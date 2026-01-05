# Low-Power 16-Bit ALU Design Using Clock Gating

## 📌 Introduction
This project presents the design and evaluation of a 16-bit Arithmetic Logic
Unit (ALU) optimized for low dynamic power consumption using clock-gating
techniques. The ALU supports 14 arithmetic, logic, shift/rotate, and comparison
operations, and is analyzed under multiple operating frequencies.

Three ALU architectures are implemented and compared to study the trade-off
between power efficiency, robustness, and hardware overhead.

---

## ⚙️ Design Variants
- **Original ALU:** Baseline design without clock gating  
- **AND-based Clock Gating:** Latch-free gating with low overhead  
- **Latch-based Clock Gating (ICG):** Glitch-free gating using enable latches  

---

## ⚡ Key Features
- **Bit-width:** 16-bit signed ALU  
- **Opcode:** 4-bit (14 supported operations)  
- **Arithmetic:** CLA addition/subtraction, Booth Radix-4 multiplication,
  restoring division  
- **Logic:** AND, OR, XOR, NAND, NOR  
- **Shift/Rotate:** Logical shift and rotate operations  
- **Power optimization:** Clock is disabled for inactive functional blocks  

---

## 🏗 System Architecture
The ALU is divided into independently gated functional units:
- Adder/Subtractor: 16-bit Carry Lookahead Adder  
- Multiplier: Booth Radix-4 (8 cycles)  
- Divider: Restoring algorithm (16 cycles)  
- Shifter/Rotator: Barrel shifter  
- Clock-gating controller driven by opcode decoding  

---

## 📊 Power Analysis Results

### Dynamic Power at 50 MHz (mW)

| Design Variant         | Dynamic Power |
|------------------------|---------------|
| Original ALU           | 5.883         |
| AND-based Clock Gating | 4.770 (–18.9%)|
| Latch-based Clock Gating | 4.961 (–15.7%)|

### Dynamic Power at 100 MHz (mW)

| Design Variant         | Dynamic Power |
|------------------------|---------------|
| Original ALU           | 7.302         |
| AND-based Clock Gating | 6.197 (–15.2%)|
| Latch-based Clock Gating | 6.607 (–9.5%) |

### Dynamic Power at 150 MHz (mW)

| Design Variant         | Dynamic Power |
|------------------------|---------------|
| Original ALU           | 8.708         |
| AND-based Clock Gating | 7.617 (–12.5%)|
| Latch-based Clock Gating | 8.245 (–5.3%) |

---

## 📐 Area Utilization

| Design Variant         | LUT | FF  | IO | BUFG |
|------------------------|-----|-----|----|------|
| Original ALU           | 395 | 204 | 72 | 1    |
| AND-based Clock Gating | 444 | 188 | 73 | 1    |
| Latch-based Clock Gating | 428 | 189 | 73 | 2    |

---

## 📝 Discussion
Clock gating significantly reduces dynamic power across all operating
frequencies. The AND-based clock-gating scheme consistently achieves the
largest power savings, particularly at higher frequencies, by aggressively
suppressing unnecessary switching in logic and I/O blocks.

The latch-based clock-gating design provides lower power savings but offers
greater immunity to clock glitches by synchronizing enable signals, making it
more suitable for glitch-sensitive or high-reliability designs.

From an area perspective, both gated designs incur moderate LUT overhead
(AND-based +12.4%, latch-based +8.4%) while reducing FF usage by approximately
7–8%. The latch-based approach requires an additional global clock buffer
(BUFG), whereas the AND-based design retains a single BUFG.

Overall, the AND-based clock-gating approach delivers the best power efficiency,
while the latch-based scheme provides a balanced trade-off between robustness
and resource utilization.

---
## References

Keating, M., Flynn, D., Aitken, R., Gibbons, A., & Shi, K. (2007). *Low power methodology manual: For system-on-chip design* (1st ed.). Springer. https://doi.org/10.1007/978-0-387-71819-4

Harris, D. M., & Harris, S. L. (2012). *Digital design and computer architecture* (2nd ed.). Morgan Kaufmann.

Chen, R. (2018, April 4). *The MIPS R4000, part 3: Multiplication, division, and the temperamental HI and LO registers*. The Old New Thing (Microsoft Dev Blogs). https://devblogs.microsoft.com/oldnewthing/

