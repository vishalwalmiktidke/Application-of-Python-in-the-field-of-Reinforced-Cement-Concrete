# Application-of-Python-in-the-field-of-Reinforced-Cement-Concrete
ASSIGNMENT NO. 05

Q.1) # To find the ultimate moment carrying capacity of singly reinforced concrete beams

# Input values
fck = float(input("Enter the value of characteristic compressive strength (MPa): "))
fy = float(input("Enter the grade of steel (MPa): "))
Es = float(input("Enter the value of Modulus of Elasticity of Steel (MPa): "))
b = float(input("Enter the value of width (mm): "))
d = float(input("Enter the value of effective depth (mm): "))
d1 = float(input("Enter the value of bar diameter (d1) (mm): "))
d2 = float(input("Enter the value of bar diameter (d2) (mm): "))
n = int(input("Enter the number of bars: "))

# Calculate area of steel	
Ast1 = n * 0.7854 * (d1 ** 2)
Ast2 = n * 0.7854 * (d2 ** 2)
print("The value of area of steel (Ast1):", Ast1, "mm²")
print("The value of area of steel (Ast2):", Ast2, "mm²")

# Total area of steel
Ast = Ast1 + Ast2
print("The total area of steel (Ast):", Ast, "mm²")

# Neutral Axis Factor
ku = 0.0035 / (0.0055 + (fy / (1.15 * Es)))
print("The value of Neutral axis factor (ku):", ku)

# Moment of Resistance factor
Ru = 0.36 * fck * ku * (1 - (0.42 * ku))
print("The value of moment of resistance factor (Ru):", Ru)

# Maximum Neutral Axis
Xumax = ku * d
print("The value of maximum neutral axis (Xumax):", Xumax)

# Actual Neutral Axis
Xu = (0.87 * fy * Ast) / (0.36 * fck * b)
print("The value of actual neutral axis (Xu):", Xu)

# Determine if the beam is under or over reinforced
if Xumax > Xu:
    print("Beam is UNDER REINFORCED")
else:
    print("Beam is OVER REINFORCED")

# Moment of Resistance Calculation
x = float(input("Enter the value of Neutral axis (x) (mm): "))

# Moment of Resistance
Mu = 0.36 * fck * x * b * (d - (0.42 * x)) * 10**-6  # Convert to kNm
print("The value of moment of resistance (Mu):", Mu, "kNm")


OUTPUT:
Enter the value of characteristic compressive strength (MPa): 20
Enter the grade of steel (MPa): 415
Enter the value of Modulus of Elasticity of Steel (MPa): 200000
Enter the value of width (mm): 230
Enter the value of effective depth (mm): 400
Enter the value of bar diameter (d1) (mm): 21
Enter the value of bar diameter (d2) (mm): 16
Enter the number of bars: 2
The value of area of steel (Ast1): 692.7228 mm²
The value of area of steel (Ast2): 402.1248 mm²
The total area of steel (Ast): 1094.8476 mm²
The value of Neutral axis factor (ku): 0.4791666666666667
The value of moment of resistance factor (Ru): 2.7556874999999996
The value of maximum neutral axis (Xumax): 191.66666666666669
The value of actual neutral axis (Xu): 238.7045446739131
Beam is OVER REINFORCED
Enter the value of Neutral axis (x) (mm): 628.32
The value of moment of resistance (Mu): 141.617593700352 kNm




Q.2) # Design of Slab

# Given Data
span = float(input("Enter the value of effective span in meters: "))
b = float(input("Enter the value of width of slab in mm: "))
bs = float(input("Enter the value of support width in meters: "))
fck = float(input("Enter the value of characteristic compressive strength (MPa): "))
fy = float(input("Enter the value of grade of steel (MPa): "))
Es = float(input("Enter the value of Modulus of Elasticity (MPa): "))
LL = float(input("Enter the value of live load (kN/m²): "))
FF = float(input("Enter the value of floor finish (kN/m²): "))
Density = float(input("Enter the value of density of RCC (kN/m³): "))

# Design Constants
# Neutral Axis Factor
ku = 0.0035 / (0.0055 + (fy / (1.15 * Es)))
print("The value of Neutral Axis Factor (ku) is:", ku)

# Moment of Resistance factor
Ru = 0.36 * fck * ku * (1 - (0.42 * ku))
print("The value of Moment Resistance factor (Ru) is:", Ru)

# Assuming pt 0.5 from fig. 04 from IS:456-2007, page no. 38
fs = float(input("Enter the value of steel stress of services (MPa): "))

# From graph find out the modification factor
MF = float(input("Enter the value of modification factor: "))

# From Clause 23.2.1, Select span/d Ratio
s = float(input("Enter the value of span/d ratio: "))

# Correction Factors
k1 = float(input("Enter the value of Correction factor if span > 10m (k1): "))
k2 = float(input("Enter the value of Tension r/f Correction factor (k2): "))
k3 = float(input("Enter the value of Compression r/f correction factor (k3): "))
k4 = float(input("Enter the value of Correction factor in case of flanged section (k4): "))

# Effective depth calculation
d1 = (span * 1000) / (s * MF * k1 * k2 * k3 * k4)
print("The value of effective depth as per deflection criteria is:", d1)

# Define Effective depth and overall depth assuming value of cover
d = float(input("Enter the value of Effective depth in mm (d): "))
D = float(input("Enter the value of overall depth in mm (D): "))

# Load Calculations
# Self Weight of Slab
DL = D * Density / 1000  # Convert to kN/m²
print("The Dead load (DL) is:", DL)

# Total Load
Factor = float(input("Enter the value of partial safety factor: "))
TL = DL + LL + FF
print("The value of total load (TL) is:", TL)
Wu = Factor * TL
print("Wu =", Wu)

# Bending Moment Calculations (Mu)
Mu = Wu * span * span / 8
print("The value of Bending Moment (Mu) is:", Mu)

# Check for Effective depth
d2 = ((Mu * 1000000) / (Ru * b))**0.5  # Convert Mu to N-mm
print("The value of Effective depth as per Moment Criteria:", d2)
if d2 > d:
    print("Revise the Depth")
else:
    print("SAFE")

print("Minimum Steel Calculations")
Astmin = 0.12 * b * D / 100
print("The value of Minimum steel area (Astmin):", Astmin)

# Main Steel Calculations
Ast = ((0.5 * fck * b * d) / (fy)) * (1 - ((1 - ((4.6 * Mu * 1000000) / (fck * b * d * d)))**0.5))  # Adjusted formula
print("Ast:", Ast)

# Check for Ast
if Ast < Astmin:
    print("Take Ast = Astmin")
else:
    print("Ast > Astmin, Hence SAFE")

Dia1 = float(input("Enter the value of bar diameter for main steel (mm): "))
Dia2 = float(input("Enter the value of bar diameter for Distribution steel (mm): "))

# Area of bar
a01 = 0.7854 * Dia1**2
print("The value of Area of main steel bar (a01):", a01)
a02 = 0.7854 * Dia2**2
print("The Value of area of distribution steel bar (a02):", a02)

# Spacing Calculations
Spacing1 = a01 * b / Ast
print("The spacing for main steel bars is:", Spacing1)
Spacing2 = a02 * b / Astmin
print("The spacing for distribution steel bars is:", Spacing2)

# Check 1 for main steel
if Spacing1 > 300:
    print("UNSAFE")
else:
    print("SAFE")

# Check 2 for main steel
if Spacing1 > 3 * d:
    print("UNSAFE")
else:
    print("SAFE")

# Check 1 for distribution steel
if Spacing2 > 300:
    print("UNSAFE")
else:
    print("SAFE")

# Check 2 for distribution steel
if Spacing2 > 5 * d:
    print("UNSAFE")
else:
    print("SAFE")

# Approximated values of spacing
S1 = float(input("Enter the values of spacing of main bars (mm): "))
S2 = float(input("Enter the values of spacing of distribution bars (mm): "))
Astprovided1 = a01 * b / S1
print("The provided steel area for main bars at section in mm² is:", Astprovided1)
Astprovided2 = a02 * b / S2
print("The provided steel area for distribution bars at section in mm² is:", Astprovided2)

# Check for shear
Vu = (Wu * span / 2) - (Wu * ((bs / 2) - (d / 1000)))
print("The value of Shear Force (SF) at section is:", Vu)
sStress = (Vu * 1000) / (b * d)  # Convert Vu to N
print("The values of shear stress is:", sStress)

# From table 20, IS:456-2007, page 73
sStressmax = float(input("Enter the value of maximum shear stress (MPa): "))
if sStress > sStressmax:
    print("Crushing will happen")
else:
    print("SAFE")

# Percentage Steel
pt = (100 * Ast) / (b * d) * 120
print("The value of percentage steel is:", pt)

# From table 19, IS:456-2007, page 73
SS = float(input("Enter the value of shear stress (MPa): "))
k = float(input("Enter the value of depth factor: "))
Shear = k * SS
print("The value of shear at section is:", Shear)
if sStress > Shear:
    print("Shear Reinforcement Required")
else:
    print("Shear Reinforcement not Required, SAFE")

# Check for Deflection
ActDEF = span * 1000 / d
print("The value of span/d is:", ActDEF)

# Actual Deflection
MaxDEF = s * MF * k1 * k2 * k3 * k4
print("The permissible deflection is:", MaxDEF)
if MaxDEF > s:
    print("UNSAFE")
else:
    print("SAFE")

# Check for Anchorage Length
M1 = 0.87 * fy * Ast * (d - ((fy * Ast) / (fck * b)))
print("The value of moment (M1):", M1)
lo = 8 * Dia1
La = 1.3 * (M1 / Vu) + 10
print("The value of anchorage length is:", La)

# Development Length
Bonds = float(input("Enter the value of bond stress (MPa): "))
Ld = 0.87 * fy * Dia1 / (4 * Bonds * s * 1.6)
print("The value of development length is:", Ld)
if La > Ld:
    print("SAFE")
else:
    print("Increase anchorage")

OUTPUT:
Enter the value of effective span in meters: 3
Enter the value of width of slab in mm: 1000
Enter the value of support width in meters: 0.23
Enter the value of characteristic compressive strength (MPa): 20
Enter the value of grade of steel (MPa): 415
Enter the value of Modulus of Elasticity (MPa): 200000
Enter the value of live load (kN/m²): 4
Enter the value of floor finish (kN/m²): 1.8
Enter the value of density of RCC (kN/m³): 25
The value of Neutral Axis Factor (ku) is: 0.4791666666666667
The value of Moment Resistance factor (Ru) is: 2.7556874999999996
Enter the value of steel stress of services (MPa): 240
Enter the value of modification factor: 1.2
Enter the value of span/d ratio: 20
Enter the value of Correction factor if span > 10m (k1): 1
Enter the value of Tension r/f Correction factor (k2): 1
Enter the value of Compression r/f correction factor (k3): 1
Enter the value of Correction factor in case of flanged section (k4): 1
The value of effective depth as per deflection criteria is: 125.0
Enter the value of Effective depth in mm (d): 130
Enter the value of overall depth in mm (D): 150
The Dead load (DL) is: 3.75
Enter the value of partial safety factor: 1.5
The value of total load (TL) is: 9.55
Wu = 14.325000000000001
The value of Bending Moment (Mu) is: 16.115625
The value of Effective depth as per Moment Criteria: 76.473082008588
SAFE
Minimum Steel Calculations
The value of Minimum steel area (Astmin): 180.0
Ast: 364.7577413804497
Ast > Astmin, Hence SAFE
Enter the value of bar diameter for main steel (mm): 10
Enter the value of bar diameter for Distribution steel (mm): 8
The value of Area of main steel bar (a01): 78.53999999999999
The Value of area of distribution steel bar (a02): 50.2656
The spacing for main steel bars is: 215.3209955264011
The spacing for distribution steel bars is: 279.25333333333333
SAFE
SAFE
SAFE
SAFE
Enter the values of spacing of main bars (mm): 210
Enter the values of spacing of distribution bars (mm): 270
The provided steel area for main bars at section in mm² is: 373.99999999999994
The provided steel area for distribution bars at section in mm² is: 186.1688888888889
The value of Shear Force (SF) at section is: 21.702375
The values of shear stress is: 0.16694134615384615
Enter the value of maximum shear stress (MPa): 2.8
SAFE
The value of percentage steel is: 33.66994535819536
Enter the value of shear stress (MPa): 0.378
Enter the value of depth factor: 1.3
The value of shear at section is: 0.4914
Shear Reinforcement not Required, SAFE
The value of span/d is: 23.076923076923077
The permissible deflection is: 24.0
UNSAFE
The value of moment (M1): 16123682.812500006
The value of anchorage length is: 965839.2079207924
Enter the value of bond stress (MPa): 1.2
The value of development length is: 23.505859374999996
SAFE
