<!DOCTYPE html>
<html lang="el">
<head>
    <meta charset="UTF-8">
    <title>Έλεγχος Τοποθεσίας</title>
    <style>
        #formContainer {
            display: none; /* Αρχικά κρύβουμε τη φόρμα */
        }
        .error {
            color: red;
        }
        .success {
            color: green;
        }
    </style>
</head>
<body>
    <h1>Έλεγχος Τοποθεσίας</h1>
    <button onclick="checkLocation()">Έλεγχος Τοποθεσίας</button>
    <div id="status"></div>
    <div id="formContainer">
        <h2>Φόρμα Παρουσίας</h2>
        <iframe id="formFrame" src="<https://forms.gle/w6FcM7gJhBFRsRed9>" width="100%" height="500px"></iframe>
    </div>

    <script>
        const universityLat = 39.620595; // Γεωγραφικό πλάτος Πανεπιστημίου Ιωαννίνων
        const universityLng = 20.853746; // Γεωγραφικό μήκος Πανεπιστημίου Ιωαννίνων
        const radius = 2.5; // Ακτίνα σε χιλιόμετρα

        function checkLocation() {
            if (navigator.geolocation) {
                navigator.geolocation.getCurrentPosition(success, error);
            } else {
                document.getElementById("status").innerText = "Η γεωγραφική τοποθεσία δεν υποστηρίζεται από τον περιηγητή σας.";
            }
        }

        function success(position) {
            const userLat = position.coords.latitude;
            const userLng = position.coords.longitude;
            const distance = getDistanceFromLatLonInKm(userLat, userLng, universityLat, universityLng);

            if (distance <= radius) {
                document.getElementById("status").innerText = "Η τοποθεσία σας έχει επιβεβαιωθεί. Είστε εντός της επιτρεπόμενης περιοχής!";
                document.getElementById("formContainer").style.display = "block"; // Εμφάνιση φόρμας
            } else {
                document.getElementById("status").innerText = "Δεν βρίσκεστε εντός της επιτρεπόμενης περιοχής. Η παρουσία δεν μπορεί να καταγραφεί.";
                document.getElementById("formContainer").style.display = "none"; // Απόκρυψη φόρμας
            }
        }

        function error() {
            document.getElementById("status").innerText = "Αδυναμία ανεύρεσης της τοποθεσίας σας.";
        }

        function getDistanceFromLatLonInKm(lat1, lon1, lat2, lon2) {
            const R = 6371; // Ακτίνα της Γης σε χιλιόμετρα
            const dLat = deg2rad(lat2 - lat1);
            const dLon = deg2rad(lon2 - lon1);
            const a =
                Math.sin(dLat / 2) * Math.sin(dLat / 2) +
                Math.cos(deg2rad(lat1)) * Math.cos(deg2rad(lat2)) *
                Math.sin(dLon / 2) * Math.sin(dLon / 2);
            const c = 2 * Math.atan2(Math.sqrt(a), Math.sqrt(1 - a));
            return R * c; // Απόσταση σε χιλιόμετρα
        }

        function deg2rad(deg) {
            return deg * (Math.PI / 180);
        }
    </script>
</body>
</html>
