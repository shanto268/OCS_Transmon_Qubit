#Goal:
An OCS qubit with $\frac{E_j}{E_c} \approx 20$

#Constraints:
	- Resonator frequency between 4 -  12 GHz (due to circulator, amplifiers and mixers)
	- $\omega_{res} \approx \omega_{03}$ of qubit 
  - $\omega_{03} = 3 \omega_{01} - 2 * α$
	- $\omega_{01}$ needs to be between 2 and 6 GHz

---

# Workflow:
1) Isolate qubit + claw design from standard candle qubit design
    a. JJ is not an element in Ansys so might need to use Lumped Element Linear Inductor with the correct $L_j$ value instead

2) Simulate (EPR) the qubit+ claw design to get $E_c  , \omega_{01}, \alpha$

    a. Ensure hyperparameters meet LL SQUILL Foundry requirements 
    b. Claws as port
    c. No other components on chip 
    d. Be careful of mesh parameters 
        i. maximum meshing size should be at least half of smallest feature
        ii. Define mesh resolution for separate components according to the rule ^
    e. Calculate $E_j$
        i. $\hbar \omega_{01} = \sqrt(8 E_j E_c) - E_c $
    f. Change simulation parameters (i.e. qubit + claw + JJ parameters) to get $E_j/E_c \approx 20$

3) Compute $\omega_{03}$

4) Simulate (S21) a claw + CPW resonator + 2-port transmission line from the standard candle qubit design

    a. Get $\omega_{res}$
        i. Fit with `lflPython/fitTools/Resonator.py` tool
    b. Change resonator length to get $\omega_{res} = \omega_{03}$

5) Choose 5 physical parameters of the qubit that have $E_j/E_c \in (10,30)$ (steps 1 and 2)

6) Get $\omega_{res}$ for these new qubits (steps 3 and 4)

7) Adjust the GDS file with the final chip design to comply with **LL SQUILL** Submission Requirements

    a. Get the JJ length for each qubit
        i. $L_j$ from $E_j$
        ii. `L_j = junction_area * critical_current_density`
        iii. `Junction_area = JJ_length * JJ_width`


