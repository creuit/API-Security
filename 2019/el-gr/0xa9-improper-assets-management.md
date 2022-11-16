API9:2019 Improper Assets Management
====================================

| Παράγοντες Απειλής (Threat agents) / Φορείς Επίθεσης (Attack vectors) | Αδυναμία Ασφαλείας (Security Weakness) | Επιπτώσεις (Impacts) |
| - | - | - |
| Εξαρτώνται από το API : Εκμεταλλευσιμότητα **3** | Επιπολασμός **3** : Ανιχνευσιμότητα **2** | Τεχνικές Επιπτώσεις **2** : Εξαρτώνται από την Επιχείρηση |
| Οι παλιές εκδόσεις ενός API μένουν συνήθως ανενημέρωτες από patches (unpatched) και έτσι αποτελούν έναν εύκολο τρόπο για την κατάληψη συστημάτων χωρίς να χρειάζεται ο εισβολέας να παλέψει με μηχανισμούς ασφαλείας τελευταίας τεχνολογίας, οι οποίοι μπορεί να υπάρχουν αλλά να προστατεύουν μόνο τις νέες εκδόσεις ενός API. | Η μη ενημερωμένη τεκμηρίωση (documentation) ενός API καθιστά πιο δύσκολη την εύρεση και/ή τη διόρθωση ευπαθειών. Η έλλειψη απογραφής περιουσιακών στοιχείων (assets inventory) και η έλλειψη στρατηγικών απόσυρσης (retire strategies) οδηγεί στο να τρέχουν ανενημέρωτα (unpatched) συστημάτα, με αποτέλεσμα τη διαρροή ευαίσθητων δεδομένων. Είναι σύνηθες να βρίσκουμε άσκοπα εκτεθειμένους κεντρικούς υπολογιστές API λόγω των σύγχρονων concepts όπως τα microservices, τα οποία καθιστούν τις εφαρμογές ανεξάρτητες και εύκολες στην ανάπτυξη (π.χ. υπολογιστικό νέφος (cloud), k8s). | Οι εισβολείς ενδέχεται να αποκτήσουν πρόσβαση σε ευαίσθητα δεδομένα ή ακόμη και να καταλάβουν τον διακομιστή μέσω παλιών, μη ενημερωμένων εκδόσεων API που συνδέονται στην ίδια βάση δεδομένων. |

## Πότε το API είναι ευάλωτο

Το API ίσως είναι ευάλωτο όταν:

* Ο σκοπός ενός κεντρικού υπολογιστή API είναι ασαφής και δεν υπάρχουν σαφείς απαντήσεις στις ακόλουθες ερωτήσεις:
  * Σε ποιο περιβάλλον εκτελείται το API (π.χ. παραγωγή (production), σταδιοποίηση (staging), δοκιμή (test), ανάπτυξη (development));
  * Ποιός πρέπει να έχει πρόσβαση δικτύου στο API (π.χ. δημόσια πρόσβαση, εσωτερική πρόσβαση, πρόσβαση σε συνεργάτες);
  * Ποια έκδοση API εκτελείται;
  * Ποια δεδομένα συλλέγονται και επεξεργάζονται από το API (π.χ. Προσωπικά αναγνωρίσιμα στοιχεία (PII));
  * Ποια είναι η ροή των δεδομένων;
* Δεν υπάρχει τεκμηρίωση (documentation) ή η υπάρχουσα τεκμηρίωση δεν έχει ενημερωθεί.
* Δεν υπάρχει σχέδιο συνταξιοδότησης (retirement plan) για κάθε έκδοση API.
* Το απόθεμα του εξυπηρετητή λείπει ή είναι ξεπερασμένο.
* Το ενσωματωμένο απόθεμα λειτουργιών, πρώτου ή τρίτου μέρους, λείπει ή είναι ξεπερασμένο. 
* Παλιές ή προηγούμενες εκδόσεις API εκτελούνται άψογα.

## Παραδείγματα Σεναρίων Επίθεσης

### Σενάριο Επίθεσης #1

Έπειτα από αναδιαμόρφωση των εφαρμογών τους, μια τοπική λειτουργία αναζήτησης άφησε μία παλιά έκδοση API (api.someservice.com/v1) να εκτελείται, απροστάτευτη, και με πρόσβαση στην βάση δεδομένων του χρήστη.  Καθώς στόχευε μία από τις τρέχουσες εκδόσεις, ένας εισβολέας βρήκε την διεύθυνση API (api.someservice.com/v2). Αντικαθιστώντας το v1 με το v2 στην διεύθυνση, ο εισβολέας απέκτησε πρόσβαση στο παλιό, απροστάτευτο API, εκθέτοντας την προσωπική αναγνωρίσιμη πληροφορία (PII) περισσότερων από 100 Εκατομμύρια χρηστών.

### Σενάριο Επίθεσης #2

Ένα κοινωνικό δίκτυο εφάρμοσε μηχανισμό περιορισμού ταχύτητας που εμποδίζει τους εισβολείς να εκτελέσουν επίθεση  ωμής βίας (brute-force) για να υπολογίσουν το διακριτικό (token) επαναφοράς συνθηματικού. Αυτός ο μηχανισμός δεν υλοποιήθηκε ως μέρος του  ίδιου κώδικα API,  αλλά σε ξεχωριστό βρόγχο μεταξύ του πελάτη και  του επίσημου API (www.socialnetwork.com). Ένας ερευνητής βρήκε έναν beta εξυπηρετητή  API (www.mbasic.beta.socialnetwork.com) που εκτελεί το ίδιο API, συμπεριλαμβανομένου του μηχανισμού επαναφοράς συνθηματικού, αλλά ο μηχανισμός περιορισμού ταχύτητας δεν υπήρχε. Ο ερευνητής ήταν ικανός να επαναφέρει το συνθηματικό οποιουδήποτε χρήστη χρησιμοποιώντας απλή ωμοβία για να υπολογίσει το διακριτικό 6 ψηφίων.

## Τρόπος Πρόληψης

* Inventory all API hosts and document important aspects of each one of them,
  focusing on the API environment (e.g., production, staging, test,
  development), who should have network access to the host (e.g., public,
  internal, partners) and the API version.
* Inventory integrated services and document important aspects such as their
  role in the system, what data is exchanged (data flow), and its sensitivity.
* Document all aspects of your API such as authentication, errors, redirects,
  rate limiting, cross-origin resource sharing (CORS) policy and endpoints,
  including their parameters, requests, and responses.
* Generate documentation automatically by adopting open standards. Include the
  documentation build in your CI/CD pipeline.
* Make API documentation available to those authorized to use the API.
* Use external protection measures such as API security firewalls for all exposed versions of your APIs, not just for the current production version.
* Avoid using production data with non-production API deployments. If this is unavoidable, these endpoints should get the same security treatment as the production ones.
* When newer versions of APIs include security improvements, perform risk analysis to make the decision of the mitigation actions required for the older version: for example, whether it is possible to backport the improvements without breaking API compatibility or you need to take the older version out quickly and force all clients to move to the latest version.

## Αναφορές (References)

### Εξωτερικές Αναφορές

* [CWE-1059: Incomplete Documentation][1]
* [OpenAPI Initiative][2]

[1]: https://cwe.mitre.org/data/definitions/1059.html
[2]: https://www.openapis.org/
