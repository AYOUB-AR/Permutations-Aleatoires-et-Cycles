#--------------------------------------------------------------------------------------------
#------------------------------------------------------------------------------
#                  Projet: Permutations aléatoires et cycles
# -----------------------------------------------------------------------------
#------------------------------------------------------------------------------

import numpy as np
import matplotlib.pyplot as plt

#------------------------------------------------------------------------------
# Préliminaire : un programme qui décompose une permutation en cycles
#------------------------------------------------------------------------------

def random_permutation(n):
    s = np.arange(0,n,dtype=int)
    for j in range(1,n):
        random_entry = np.random.randint(0,n-j+1)
        old_value = s[n-j]
        s[n-j] = s[random_entry]
        s[random_entry]=old_value
    return s+1


#------------------------------------------------------------------------------
#---------------Question 1: VA qui donne la longueur du cycle leplus long------
#------------------------------------------------------------------------------


def cycles(P):
    
  mem = set(P)
  result = []
  while len(mem) > 0:
    m = mem.pop()
    cycle = [m]
    
    while True:
      m = P[m-1]
      if m not in mem:
        break
      mem.remove(m)
      cycle.append(m)
    result.append(cycle)
  return result
  

def len_cycle_max(Tab):
    k = 1
    n = len(Tab)
    save = len(Tab[0]) 
    while k < n :   
        if len(Tab[k]) > save:
            save = len(Tab[k])
            k += 1
        else:
            k += 1
    return save 


def X1(n,N):
    k = 0
    X = np.zeros(N)
    while k < N : 
        Perm = random_permutation(n)
        cycle = cycles(Perm)
        X[k] = len_cycle_max(cycle)
        k += 1
    return X


Tabx = X1(100,1000)

plt.title("Observation de X")
plt.plot(Tabx,'r--')
plt.savefig('CourbeX.png')  # ficher dans zip
plt.close()



plt.title("Histogramme de X")
plt.hist(Tabx,bins='auto',density=True)
plt.savefig('histX1.png')  # ficher dans zip
plt.close()




#------------------------------------------------------------------------------
#---------------Question 2: Proba d etre dans le meme cycle--------------------
#------------------------------------------------------------------------------
 


def Kpoints(n,k):
    Tab = random_permutation(n)
    return Tab[:k]


def search_points(Tab0,Tab1):
    N = len(Tab0)
    n = len(Tab1)
    i = 0
    
    while i < N:
        if np.sum(np.in1d(Tab0[i],Tab1)) == n :
            return True 
        i += 1
    return False 
   
def prob_points_include(n,k,N):
    points = Kpoints(n,k)
    prob = []
    for j in range(0,N):
        Tab = random_permutation(n)
        cycle = cycles(Tab)
        A = search_points(cycle,points)
        prob.append(A)
    
    return np.mean(prob)
    
#Exemple :
print(prob_points_include(100,3,10000))




#------------------------------------------------------------------------------
#---------------Question 4: VA qui compte les nombres de cycles----------------
#------------------------------------------------------------------------------



def X2(n):
    P = random_permutation(n)
    cycle = cycles(P)
    return len(cycle)

N = 10000
n= 100

tabX = []
for j in range(1,N):
    tabX.append(X2(n))


plt.title("Histogramme de X2")
plt.hist(tabX,bins='auto',density=True)
plt.savefig('histX2.png')   # ficher dans zip
plt.close()
