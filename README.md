
### **Day 38: Assertions – Basics in SystemVerilog**

#### **1. Overview**

**Assertions** in SystemVerilog are used to **verify correctness of behavior** during simulation. They help check that certain conditions hold true during the simulation of a design, and if not, they flag errors.

Assertions are a powerful way to **catch design bugs early**, especially those that might go unnoticed with only waveform inspection.

---

#### **2. Types of Assertions in SystemVerilog**

SystemVerilog supports two main types:

1. **Immediate Assertions** – evaluated immediately when encountered in simulation (procedural code).
2. **Concurrent Assertions** – used to express time-based properties (monitored over time).

In today's session, we focus on **Immediate Assertions**.

---

#### **3. Immediate Assertions**

**Syntax:**

```systemverilog
assert (<expression>) else <action_if_false>;
```

- Evaluated instantly when the simulation reaches that point.
- Used inside `initial`, `always`, `functions`, and `tasks`.

**Example:**

```systemverilog
initial begin
    a = 1; b = 0;
    #10;
    assert (a == 1) else $error("Assertion failed: a != 1");
end
```

---

#### **4. Use Cases of Immediate Assertions**

- Checking signal values at specific simulation times
- Validating output values for given inputs
- Ensuring protocol rules are followed at each cycle
- Making simulation self-checking

---

#### **5. Detailed Example:**

Let’s say we have a multiplexer design:

```systemverilog
module mux2 (
    input  logic sel,
    input  logic a,
    input  logic b,
    output logic y
);
    always_comb begin
        if (sel)
            y = b;
        else
            y = a;
    end
endmodule
```

**Testbench with Assertions:**

```systemverilog
module tb_mux2;

    logic sel, a, b, y;

    mux2 dut (
        .sel(sel),
        .a(a),
        .b(b),
        .y(y)
    );

    initial begin
        sel = 0; a = 1; b = 0; #10;
        assert (y == a) else $error("Test failed when sel=0");

        sel = 1; a = 1; b = 1; #10;
        assert (y == b) else $error("Test failed when sel=1");

        sel = 1; a = 0; b = 1; #10;
        assert (y == b) else $fatal("Assertion failed: sel=1, y != b");

        $display("All assertions passed.");
        $finish;
    end

endmodule
```

---

#### **6. Assertion Severity Levels**

SystemVerilog provides these functions to handle failed assertions:

| Severity Level | Action                       |
|----------------|------------------------------|
| `$error`       | Prints message, continues    |
| `$warning`     | Prints warning, continues    |
| `$info`        | Just prints information      |
| `$fatal`       | Stops simulation             |

You can choose based on how critical the assertion is.

---

#### **7. Best Practices for Immediate Assertions**

- Use **descriptive messages** to help locate the failure.
- Place assertions **near relevant logic** or stimulus.
- Use `$fatal` for **critical checks** that must not fail.
- Do not overuse assertions for things that aren’t violations.
- Keep assertions **simple and specific**.

---

#### **8. Benefits of Using Assertions**

- Catch bugs **immediately at the point of error**
- Reduce reliance on **manual waveform inspection**
- Improve **debug efficiency**
- Help build **self-checking testbenches**
- Enhance **design documentation** (asserts describe intent)

---

#### **9. Summary**

- Immediate assertions are **procedural** and are checked at simulation time.
- Syntax is simple: `assert (condition) else action`.
- Ideal for **basic value checks** and small-scale verification.
- Assertions improve the **quality and readability** of both design and testbenches.
- Tomorrow we’ll cover **Concurrent Assertions**, which are more powerful for **time-based and protocol checks**.
