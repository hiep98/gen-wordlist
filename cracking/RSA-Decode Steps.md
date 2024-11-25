QUESTION:
But once again, you got caught; your SANS teacher knows their way around algorithms.
Having noticed your interest in Cryptography, your instructor has decided to enrol you in a SANS Cryptography summer course. They are willing to give you one last chance!

Knowing your love for exam environments, your instructor loses no time and gives you this encrypted message:

394915266259123698656085326685566309096988250169854318775283879050626940299248731770121310771261917272983959978907239241459313798553627277173943769698049331941588651718098116260006208254801900246563345683201685047177960888144918519835475380588613717488256926418949737981637305866982218074491572749448554200909288217277926434968862216254016095751351456571568154479784253763216175804600621409973962612015516458205645270619071419977379451572650918410949423395185246199349815215892498267023208580493930
Can you figure it out and make amends for all of the cheating?

Download the encryption script using the AttackBox from this URL http://10.10.133.71/Easy-RSA.py, find the decryption key and get those answers before the exam ends.

from Crypto.Util.number import getPrime, bytes_to_long

flag = flag{REDACTED}
n = 408579146706567976063586763758203051093687666875502812646277701560732347095463873824829467529879836457478436098685606552992513164224712398195503564207485938278827523972139196070431397049700119503436522251010430918143933255323117421712000644324381094600257291929523792609421325002527067471808992410166917641057703562860663026873111322556414272297111644069436801401012920448661637616392792337964865050210799542881102709109912849797010633838067759525247734892916438373776477679080154595973530904808231
ct = pow(bytes_to_long(flag),65537,n)

print(f"Ciphertext: {ct}")




ANWSER:

from Crypto.Util.number import inverse
from sympy import factorint

# The modulus n from the original problem (replace with actual value)
n = 408579146706567976063586763758203051093687666875502812646277701560732347095463873824829467529879836457478436098685606552992513164224712398195503564207485938278827523972139196070431397049700119503436522251010430918143933255323117421712000644324381094600257291929523792609421325002527067471808992410166917641057703562860663026873111322556414272297111644069436801401012920448661637616392792337964865050210799542881102709109912849797010633838067759525247734892916438373776477679080154595973530904808231

# The ciphertext ct from the original problem (replace with actual value)
ct = 394915266259123698656085326685566309096988250169854318775283879050626940299248731770121310771261917272983959978907239241459313798553627277173943769698049331941588651718098116260006208254801900246563345683201685047177960888144918519835475380588613717488256926418949737981637305866982218074491572749448554200909288217277926434968862216254016095751351456571568154479784253763216175804600621409973962612015516458205645270619071419977379451572650918410949423395185246199349815215892498267023208580493930

# Step 1: Factorize n using sympy.factorint
factors = factorint(n)

# If n is semiprime, it should have two factors (p and q)
if len(factors) == 2:
    p, q = list(factors.keys())  # Extract p and q
else:
    raise ValueError("Failed to factor n into two primes")

# Step 2: Compute phi(n) = (p - 1) * (q - 1)
phi_n = (p - 1) * (q - 1)

# Step 3: Define RSA public exponent e (usually 65537)
e = 65537

# Step 4: Compute the private exponent d using the modular inverse of e mod phi(n)
d = inverse(e, phi_n)

# Step 5: Decrypt the ciphertext
decrypted_flag = pow(ct, d, n)

# Step 6: Convert the decrypted number to bytes
# Convert the decrypted number to bytes
num_bytes = (decrypted_flag.bit_length() + 7) // 8  # Number of bytes required
flag_bytes = decrypted_flag.to_bytes(num_bytes, byteorder='big')

# Step 7: Decode the bytes to get the flag
flag = flag_bytes.decode('utf-8')

# Print the flag
print("Flag:", flag)


----

from Crypto.Util.number import inverse, long_to_bytes
from sympy import factorint

n = 408579146706567976063586763758203051093687666875502812646277701560732347095463873824829467529879836457478436098685606552992513164224712398195503564207485938278827523972139196070431397049700119503436522251010430918143933255323117421712000644324381094600257291929523792609421325002527067471808992410166917641057703562860663026873111322556414272297111644069436801401012920448661637616392792337964865050210799542881102709109912849797010633838067759525247734892916438373776477679080154595973530904808231
ct = 394915266259123698656085326685566309096988250169854318775283879050626940299248731770121310771261917272983959978907239241459313798553627277173943769698049331941588651718098116260006208254801900246563345683201685047177960888144918519835475380588613717488256926418949737981637305866982218074491572749448554200909288217277926434968862216254016095751351456571568154479784253763216175804600621409973962612015516458205645270619071419977379451572650918410949423395185246199349815215892498267023208580493930

factors = factorint(n)  # This returns a dictionary {prime: exponent}
p, q = factors.keys()

phi_n = (p - 1) * (q - 1)

e = 65537
d = inverse(e, phi_n)

m = pow(ct, d, n)

flag = long_to_bytes(m)
print(flag.decode())