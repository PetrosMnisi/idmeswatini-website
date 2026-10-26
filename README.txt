IDM ESWATINI WEBSITE  (PHP 8 + MySQL) — updated 26 October 2026
================================================================

The website is fully dynamic: pages, programmes, programme structures, short courses, the staff directory,
curricula, news, settings, applications and the intranet all come from ONE MySQL database. Personal data is
encrypted in the database (AES-256-GCM) and only shown to the people allowed to see it, after they log in.

1. INSTALL ON XAMPP (this computer)
   - Start Apache and MySQL in the XAMPP Control Panel.
   - phpMyAdmin (http://localhost/phpmyadmin) > New > create the database  idm_eswatini_website
     > select it > Import > database/idm_eswatini_website.sql > Go.
   - Open http://localhost/idm-eswatini-website/

2. INSTALL ON ANY WEB HOST (cPanel, Plesk, DirectAdmin, hPanel, Windows/IIS, Nginx)
   - Requirements: PHP 8.0 or newer with PDO MySQL and OpenSSL; MySQL 5.7+/8 or MariaDB 10.3+.
   - In the hosting panel create a MySQL database and a database user with all privileges on it.
   - phpMyAdmin > select that database > Import > database/idm_eswatini_website.sql > Go.
     (The file does not create or select a database itself, so it imports into the one you selected.)
   - Edit inc/config.php: 'host' (usually localhost), 'name', 'user', 'pass' and 'site_url'
     (e.g. https://www.idmbls.ac.sz). Fill in 'mail' to send emails to applicants.
   - Upload ALL website files EXCEPT the database/ folder, _backup/, _drafts/ and "Claude outputs/".
     inc/secret-key.php MUST be uploaded: it decrypts the personal data. Keep a backup copy offline.
   - Make uploads/ and private_docs/ writable (755 or 775).
   - Apache/LiteSpeed (2.2 and 2.4) use .htaccess, Windows/IIS uses web.config; on Nginx paste nginx-example.conf.txt
     into the server { } block above the PHP location. Program code lives in app/ and inc/, which are never served.
   - Tested on Apache 2.4 (main folder and sub-folder), Nginx + PHP-FPM and the PHP built-in server, PHP 8.3.
   - Optional: open setup.php (as an Administrator) to check the server.

3. ADMISSIONS WORKFLOW (automatic routing)
   Applicant applies (1st and 2nd choice, country > curriculum > subjects and grades)
   -> Admissions verifies the documents
   -> Finance verifies the proof of payment
   -> the application goes AUTOMATICALLY to the Head of Department of the 1st-choice programme
   -> HOD ADMITS: student number + acceptance letter generated, applicant emailed, letter in the portal
      HOD DOES NOT ADMIT (reason required): applicant notified with the reason, and the application goes
      AUTOMATICALLY to the HOD of the 2nd-choice programme. If that HOD also does not admit (or there is
      no 2nd choice) the outcome is "Not admitted" with both reasons.
   The applicant portal always shows "Where your application is now" (Admissions, Finance, which HOD...),
   and the applicant can print the application (Print my application).
   Heads of Department: BIRM Mncedisi Msibi, PHM Nomcebo Dlamini, HRM Lucia Mkhaliphi, Education Nelisiwe Ndlela.

4. WORK QUEUES AND REPORTS (processing.php, "Work queue & reports" in every staff portal)
   - HODs see the applications waiting for their decision; Finance the payments to verify; Admissions the
     documents to check.
   - The Executive Director (Mr Karabo Baipidi), the Assistant Executive Director / ADAS (Dr Bongani
     Dlamini), the Registrar and Administrators see the whole pipeline, waiting times, overdue items and
     each HOD's queue.
   - Applicants report: filter by country, curriculum, year of completion, number of credits and passes,
     English/Maths credit, department, programme, status, stage, gender, region, intake and dates; group by
     any of these; print / save as PDF or download for Excel. Click a reference for the printable
     single-applicant report.

5. CURRICULA
   Countries, curricula, subjects and grading scales are in the database (documents: site / curricula).
   Built-in: Eswatini (EGCSE, SGCSE, IGCSE, COSC, AS/A Level), South Africa (NSC, IEB, NC(V)), Mozambique,
   Botswana, Lesotho, Namibia, Zimbabwe, Zambia, Malawi, Kenya, Nigeria/West Africa (WASSCE), UK (GCSE,
   A Level), International (IB, IGCSE) and Other. The server checks every grade against the chosen scale and
   counts credits, passes and fails itself.

6. STAFF INTRANET (intranet.php)
   Policies (search inside PDFs, acknowledgements, versions), notices, My work, Staff directory,
   Service requests (ICT helpdesk, facilities, HR, finance, registry, library, with email updates) and
   Quick links to every system.

7. NAVIGATION
   Cascading menus everywhere (Academics > Departments > department > level > programme). Every link shows
   its real address (for example #programmes/prog:c1, #about/history), so pages can be bookmarked and shared.
   The Academics page has a cascading Academic navigator.

8. SECURITY NOTES
   - Staff log in at login.php, applicants at applicant-login.php. Passwords are stored only as hashes.
   - Default passwords are issued privately by ICT; everyone must choose their own at first login.
   - Remove the SAMPLE applications (emails @example.com) before going live:
     Administration console > System & activity > Remove sample applicants.
   - Old file inc/secret.key is no longer needed once inc/secret-key.php is in place (same key).
