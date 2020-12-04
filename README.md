# benchmarks
# ΑΡΧΙΤΕΚΤΟΝΙΚΗ ΠΡΟΗΓΜΕΝΩΝ ΥΠΟΛΟΓΙΣΤΩΝ


## Εργαστήριο Β' - Ομάδα 14
 
* Παπαδόπουλος Σωτήριος (Α.Ε.Μ. 4822, <swtos@ece.auth.gr>)
* Σαββάογλου Δημήτριος  (Α.Ε.Μ. 9388, <dsavvaog@ece.auth.gr>)

### **2η Εργαστηριακή Άσκηση: Design Space Exploration με τον _gem5_**


  Το θέμα της 2ης εργαστηριακής άσκησης είναι η εκτέλεση μιας σειράς από benchmarks στον _gem5_ και η εξαγωγή συμπερασμάτων από τα αποτελέσματα. Αυτό θα μας βοηθήσει στην επιλογή κάποιων χαρακτηριστικών για τον σχεδιασμό του επεξεργαστή σε συνδυασμό πάντα με το κόστος κατασκευής του.


**ΠΡΟΕΡΓΑΣΙΑ:**

  Κατεβάσαμε τα benchmarks (ένα υποσύνολο των _SPEC cpu2006 benchmarks_) και εκτελέσαμε το compile χρησιμοποιώντας τους _ARM cross compilers_ που είχαμε εγκαταστήσει από την 1η εργαστηριακή άσκηση στο σύστημά μας. Στη συνέχεια εκτελέσαμε τα benchmarks αυτά χρησιμοποιώντας τις εντολές της εκφώνησης. Για κάθε benchmark δημιουργήθηκε και ένας αντίστοιχος φάκελος με κάποια αρχεία από τα οποία εξαγάγαμε τα συμπεράσματά μας.


**ΕΡΩΤΗΜΑΤΑ:**

**Βήμα 1ο**

**Ερώτημα 1:** Οι βασικές παράμετροι για τον επεξεργαστή που εξομοιώνει ο _gem5_ όσον αφορά το υποσύστημα μνήμης είναι οι εξής:

   -  _L1 instruction cache_: 64KB, associativity 2

   -  _L1 data cache_: 32KB, associativity 2

   -  _L2 cache_: 2MB, associativity 8

   -  _cache line size_: 64 bytes

**Ερώτημα 2:**

   -_specbzip_: i) 78,875ms ii) 1,5775 cpi iii) D-cache 0,012718, I-cache 0,00007, L2 cache 0,332104

   -_specmcf_: i) 58,373ms ii) 1,167452 cpi iii) D-cache 0,00207, I-cache 0,007157, L2 cache 0,156178

   -_spechmmer_: i) 58,088ms ii) 1,161762 cpi iii) D-cache 0,001782, I-cache 0,000154, L2 cache 0.076981

   -_specsjeng_: i) 516,572ms ii) 10,33145 cpi iii) D-cache 0,122, I-cache 0,000021, L2 cache 0,99997

   -_speclibm_: i) 175,15ms ii) 3,503 cpi iii) D-cache 0,060972, I-cache 0,00009, L2 cache 0,999961






Παρατηρήσεις:

Τα γραφήματα 1 και 2 είναι σχεδόν πανομοιότυπα, πράγμα που σημαίνει ότι υπάρχει αναλογία μεταξύ του χρόνου εκτέλεσης και του CPI για κάθε benchmark.
Το miss rate για την L1 I-cache είναι σημαντικά μικρό στα περισσότερα benchmarks, ενώ αντίστοιχα σχετικά μεγάλο για την L2 cache. Συμπεραίνουμε λοιπόν ότι η L2 cache είναι κατά κύριο λόργο υπεύθυνη για την όποια καθυστέρηση στην εκτέλεση των benchmarks.


**Ερώτημα 3:**

Παίρνουμε τις εξής τιμές: 

_system.clk_domain.clock_: 1000 ticks οπότε  1000000000000/1000=**1GHz**

_system.cpu_clk_domain.clock_: 500 ticks οπότε 1000000000000/500=**2GHz**

Αν προσθέσουμε άλλον έναν επεξεργαστή η συχνότητά του θα είναι 2GHz.

Για το benchmark _spebzip_ ο χρόνος εκτέλεσης είναι 151,578ms (για 1GHz). Δεν υπάρχει τέλειο scaling είναι όμως ικανοποιητικό.


**Βήμα 2ο**

Μετά από δοκιμές επελέγησαν οι εξής τιμές για την ελαχιστοποίηση του CPI:

   -_specbzip_: L1 D-cache size 128KB, associativity 16, I-cache size 128KB, associativity 2, L2 cache size 4MB, associativity 512, cache line size 128 bytes. Με αυτές τις παραμέτρους πήραμε τιμή για το CPI 1,525502.

   -_specmcf_: L1 D-cache size 128KB, associativity 16, I-cache size 128KB, associativity 4, L2 cache size 4MB, associativity 1024, cache line size 128 bytes. Με αυτές τις παραμέτρους πήραμε τιμή για το CPI 1,099814.

   -_spechmmer_: L1 D-cache size 128KB, associativity 16, I-cache size 128KB, associativity 16, L2 cache size 2MB, associativity 8, cache line size 128 bytes. Με αυτές τις παραμέτρους πήραμε τιμή για το CPI 1,152215.

   -_specsjeng_: L1 D-cache size 128KB, associativity 16, I-cache size 128KB, associativity 16, L2 cache size 4MB, associativity 512, cache line size 128 bytes. Με αυτές τις παραμέτρους πήραμε τιμή για το CPI 6,826205.

   -_speclibm_: L1 D-cache size 128KB, associativity 16, I-cache size 128KB, associativity 16, L2 cache size 4MB, associativity 2048, cache line size 128 bytes. Με αυτές τις παραμέτρους πήραμε τιμή για το CPI 2,555222.

Παρατηρήθηκε ότι σε κάποιες περιπτώσεις η τιμή της L2 cache όπως επίσης και το αντίστοιχο associativity δεν επηρεάζουν την τιμή του CPI οπότε σε αυτήν την περίπτωση η τιμή της L2 cache παρέμεινε η προεπιλεγμένη (2MB size, assosiativity 8).






**Βήμα 3ο**



**Πηγές:**

   www.gem5.org

   www.m5sim.org

   www.sciencedirect.com/topics/computer-science/set-associativity
