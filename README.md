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

   -_specbzip_: i. 78,875ms ii. 1,5775 cpi iii. D-cache 0,012718, I-cache 0,00007, L2 cache 0,332104 για 2GHz1 (i. 151,578ms ii. 1,515775 cpi iii. D-cache 0,012517, I-cache 0,00007, L2 cache 0,332094 για 1GHz)   

   -_specmcf_: i. 58,373ms ii. 1,167452 cpi iii. D-cache 0,00207, I-cache 0,007157, L2 cache 0,156178 για 2GHz (i. 113,928ms ii. 1,139281 cpi iii. D-cache 0,00207, I-cache 0,005227, L2 cache 0,198367 για 1GHz)

   -_spechmmer_: i. 58,088ms ii. 1,161762 cpi iii. D-cache 0,001782, I-cache 0,000154, L2 cache 0.076981 για 2GHz

   -_specsjeng_: i. 516,572ms ii. 10,33145 cpi iii. D-cache 0,122, I-cache 0,000021, L2 cache 0,99997 για 2GHz (i. 709,163ms ii. 7,019634 cpi iii. D-cache 0,121997, I-cache 0,000021, L2 cache 0,99997 για 1GHz)

   -_speclibm_: i. 175,15ms ii. 3,503 cpi iii. D-cache 0,060972, I-cache 0,00009, L2 cache 0,999961 για 2GHz (i. 262,251ms ii. 2,622513 cpi iii. D-cache 0,060792, I-cache 0,00009, L2 cache 0,999961 για 1GHz)


![Pie Charts](/plot.PNG)


Παρατηρήσεις:

Τα γραφήματα 1 και 2 είναι σχεδόν πανομοιότυπα, πράγμα που σημαίνει ότι υπάρχει αναλογία μεταξύ του χρόνου εκτέλεσης και του CPI για κάθε benchmark.
Το miss rate για την L1 I-cache είναι σημαντικά μικρό στα περισσότερα benchmarks, ενώ αντίστοιχα σχετικά μεγάλο για την L2 cache. Συμπεραίνουμε λοιπόν ότι η L2 cache είναι κατά κύριο λόγο υπεύθυνη για την όποια καθυστέρηση στην εκτέλεση των benchmarks.


**Ερώτημα 3:**

Παίρνουμε τις εξής τιμές: 

_system.clk_domain.clock_: 1000 ticks οπότε  1000000000000/1000=**1GHz**

_system.cpu_clk_domain.clock_: 500 ticks οπότε 1000000000000/500=**2GHz**

Αν προσθέσουμε άλλον έναν επεξεργαστή η συχνότητά του θα είναι 2GHz.

Για το benchmark _spebzip_ ο χρόνος εκτέλεσης είναι 151,578ms (για 1GHz). Δεν υπάρχει τέλειο scaling είναι όμως ικανοποιητικό. Αυτό πιθανόν να οφείλεται σε αστοχίες του hardware ή του software οι οποίες είναι πιο δύσκολο να προβλεφθούν και να ελεγχθούν.


**Βήμα 2ο**

Μετά από δοκιμές επελέγησαν οι εξής τιμές για την ελαχιστοποίηση του CPI:

   -_specbzip_: L1 D-cache size 128KB, associativity 16, I-cache size 32KB, associativity 2, L2 cache size 4MB, associativity 8, cache line size 128 bytes. Με αυτές τις παραμέτρους πήραμε τιμή για το CPI 1,527291 (λόγος βελτίωσης 1,0329).

   -_specmcf_: L1 D-cache size 64KB, associativity 2, I-cache size 64KB, associativity 4, L2 cache size 2MB, associativity 8, cache line size 64 bytes. Με αυτές τις παραμέτρους πήραμε τιμή για το CPI 1,142269 (λόγος βελτίωσης 1,022).

   -_spechmmer_: L1 D-cache size 128KB, associativity 4, I-cache size 32KB, associativity 2, L2 cache size 2MB, associativity 8, cache line size 128 bytes. Με αυτές τις παραμέτρους πήραμε τιμή για το CPI 1,152215 (λόγος βελτίωσης 1,0083).

   -_specsjeng_: L1 D-cache size 64KB, associativity 2, I-cache size 32KB, associativity 2, L2 cache size 2MB, associativity 8, cache line size 128 bytes. Με αυτές τις παραμέτρους πήραμε τιμή για το CPI 6,837885 (λόγος βελτίωσης 1,511).

   -_speclibm_: L1 D-cache size 64KB, associativity 2, I-cache size 32KB, associativity 2, L2 cache size 4MB, associativity 8, cache line size 128 bytes. Με αυτές τις παραμέτρους πήραμε τιμή για το CPI 2,577035 (λόγος βελτίωσης 1,3593).

Παρατηρήθηκε ότι σε κάποιες περιπτώσεις η τιμή της L2 cache όπως επίσης και το αντίστοιχο associativity δεν επηρεάζουν την τιμή του CPI οπότε σε αυτήν την περίπτωση η τιμή της L2 cache παρέμεινε η προεπιλεγμένη (2MB size, assosiativity 8).


![Pie Charts](/diagram.PNG)
![Pie Charts](/diagram_2.PNG)


   Παρατηρώ ότι κάθε παράμετρος επηρεάζει την απόδοση το πολύ των δύο από τα τέσσερα benchmarks. Για αυτό το λόγο χρησιμοποιούνται διαφορετικοί συνδυασμοί για κάθε benchmark ώστε να επιτύχουμε μέγιστη απόδοση. Αυτό σημαίνει ότι δεν υπάρχει συγκεκριμένη αρχιτεκτονική η οποία να δίνει ελάχιστο CPI για οποιοδήποτε πρόγραμμα τρέχει σε αυτήν, αλλά το CPI (και κατ' επέκταση ο χρόνος εκτέλεσης) ενός προγράμματος εξαρτάται κυρίως από το ίδιο το πρόγραμμα. Ως εκ τούτου υπάρχει όριο στη μείωση του CPI στα benchmarks τα οποία τρέξαμε, αλλά και αισθητή διαφορά του CPI μεταξύ αυτών.


![Pie Charts](/bzip.png)


**Βήμα 3ο**


   Ας υποθέσουμε ότι διπλασιασμός του μεγέθους μιας cache μνήμης σημαίνει αύξηση 30% του κόστους. Στην περίπτωσή μας διαθέτουμε δύο L1 caches της τάξης των εκατοντάδων KB, οπότε ας θεωρήσουμε (σχετικό) κόστος υλοποίησης ίσο με 1 για την καθεμία, οπότε συνολικά 2 μονάδες κόστους. Η L2 cache από την άλλη που αποτελεί το σύστημά μας έχει μέγεθος της τάξης των MB, οπότε το σχετικό κόστος υπολογίζεται επίσης στη 1 μονάδα περίπου. Επομένως θεωρούμε 3 μονάδες κόστους για όλο το σύστημα.
   Επίσης το associativity επηρεάζει την πολυπλοκότητα της cache μνήμης, διότι υψηλό associativity σημαίνει μεγάλη πολυπλοκότητα. Πχ για τις L1 caches με συγκεκριμένο cache line size και σταθερό μέγεθος κύριας μνήμης διπλασιασμός του associativity σημαίνει διπλάσιο πλήθος γραμμών, οπότε θεωρούμε αύξηση του κόστους περίπου 10%. Συνολικά θεωρούμε (1.1^log2n)xL1 I-cache line size cost + (1.1^log2m)xL1 D-cache size cost + (1.1^log2l)xL2 cache size cost, για n, m ή l-way associative caches αντίστοιχα οι L1 I-cache, L1 D-cache και L2 cache. Τέλος, διπλασιασμός του cache line size (πχ 128 αντί 64 bytes) για το ίδιο associativity σημαίνει μεν μισό πλήθος cache lines, δεν αλλάζει δε το πλήθος γραμμών, οπότε δεν επηρεάζεται το κόστος υλοποίησης.

Όσον αφορά το κόστος για κάθε ξεχωριστό benchmark, υποθέτουμε ότι για τις default τιμές των παραμέτρων έχουμε κόστος ίσο με 3:

specbzip: Σύμφωνα με τις τιμές των παραμέτρων που επιλέξαμε στο βήμα 2, το κόστος υπολογίζεται ως εξής: (1.1^3)x1.3 + (1.1^0)x1 + (1.1^0)x1.3 = 4,0303 μονάδες
specmcf: (1.1^0)x1 + (1.1^1)x1.3 + (1.1^0)x1 = 3,43 μονάδες
spechmmer: (1.1^1)x1.3 + (1.1^0)x1 + (1.1^0)x1 = 4,9797 μονάδες
specsjeng: (1.1^0)x1 + (1.1^0)x1 + (1.1^0)x1 = 3,43 μονάδες 
speclibm: (1.1^0)x1 + (1.1^0)x1 + (1.1^0)x1,3 = 3,3 μονάδες

   Παρατηρούμε ότι το κόστος υλοποίησης των αρχιτεκτονικών που βελτιστοποιούν την απόδοση για κάθε benchmark είναι από 3,5 έως 5 μονάδες. Έτσι διαπιστώνουμε ότι στις τέσσερις από τις πέντε περιπτώσεις η αποδοτικότερη αρχιτεκτονική είναι περισσότερο κοστοβόρα. Ένας συμβιβασμός στην περίπτωση του speclibm είναι να κρατήσουμε τα 2MB για την L2 cache, μια και τα 4MB μειώνουν ελάχιστα το CPI του συστήματος όπως δείχνει το αντίστοιχο διάγραμμα. Έτσι θα έχω κόστος 3 μονάδες, δηλαδή ίσο με το αρχικό.




















**Πηγές:**

www.gem5.org

www.m5sim.org

www.sciencedirect.com/topics/computer-science/set-associativity













Σημείωση 1: Στους εξομοιωτές και των δύο φοιτητών η default τιμή για το cpu clock ήταν τα 2GHz.
