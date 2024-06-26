---
title: Αναζήτηση Αναγνωριστικών Λογαριασμού
slug: account-id
date: 2016-08-10
lastmod: 2019-07-30
show_lastmod: 1
---


Όταν αναφέρετε προβλήματα, μπορεί να είναι χρήσιμο να παρέχετε το ID του λογαριασμού Let's Encrypt. Τις περισσότερες φορές, η διαδικασία δημιουργίας ενός λογαριασμού αντιμετωπίζεται αυτόματα από το λογισμικό πελάτη ACME που χρησιμοποιείτε για να μιλήσετε στο Let's Encrypt, και μπορεί να έχετε πολλαπλούς λογαριασμούς ρυθμισμένους αν τρέξετε τους πελάτες ACME σε πολλούς διακομιστές.

Το ID λογαριασμού σας είναι μια διεύθυνση URL της φόρμας `https://acme-v02.api.letsencrypt.org/acme/acct/12345678`.

Αν χρησιμοποιείτε το Certbot, μπορείτε να βρείτε το ID του λογαριασμού σας κοιτάζοντας το πεδίο "uri" στο `/etc/letsencrypt/accounts/acme-v02.api.letsencrypt.org/directory/*/regr.json`.

Αν χρησιμοποιείτε ένα άλλο πρόγραμμα ACME, οι οδηγίες θα εξαρτώνται από τον πελάτη. Ελέγξτε τα αρχεία καταγραφής σας για URL της φόρμας που περιγράφεται παραπάνω. Εάν ο πελάτης ACME σας δεν καταγράφει το ID λογαριασμού, μπορείτε να το ανακτήσετε υποβάλλοντας μια νέα αίτηση εγγραφής με το ίδιο κλειδί. Δείτε το [ACME spec για περισσότερες λεπτομέρειες](https://tools.ietf.org/html/rfc8555#section-7.3). Μπορείτε επίσης να βρείτε την αριθμητική μορφή του αναγνωριστικού σας στην κεφαλίδα του αιτήματος Boulder-Requester την απάντηση σε κάθε POST πελάτη ACME που κάνει.
