# orange — Ubuntu practical steps (CC CSA-302)

Open terminal: **Ctrl+Alt+T**

## A. MySQL

### 1. Enter MySQL
```
sudo mysql
```
(Not installed? `sudo apt install mysql-server -y`)

### 2. workshopdb
```sql
CREATE DATABASE workshopdb;
USE workshopdb;
CREATE TABLE Registration(RegistrationID INT PRIMARY KEY, ParticipantName VARCHAR(50), Department VARCHAR(30));
INSERT INTO Registration VALUES (1,'Jason','BCA'),(2,'Riya','BBA');
SELECT * FROM Registration;
SELECT * FROM Registration WHERE Department='BCA';
```

### 3. inventorydb
```sql
CREATE DATABASE inventorydb;
USE inventorydb;
CREATE TABLE Asset(AssetID INT PRIMARY KEY, AssetName VARCHAR(50), Quantity INT);
INSERT INTO Asset VALUES (1,'Mouse',10),(2,'Keyboard',3),(3,'Monitor',8);
SELECT * FROM Asset WHERE Quantity > 5;
```

### 4. Q11 — user with limited permissions
```sql
CREATE USER 'wsuser'@'localhost' IDENTIFIED BY 'Pass@1234';
GRANT SELECT, INSERT ON workshopdb.* TO 'wsuser'@'localhost';
FLUSH PRIVILEGES;
SHOW GRANTS FOR 'wsuser'@'localhost';
exit
```
```
mysql -u wsuser -p
```
```sql
USE workshopdb;
INSERT INTO Registration VALUES (3,'Aman','BCA');   -- works
SELECT * FROM Registration;                          -- works
DELETE FROM Registration WHERE RegistrationID=3;     -- ERROR: denied
exit
```

### Fix mistakes
```sql
RENAME TABLE wrongname TO Registration;
ALTER TABLE Registration RENAME COLUMN wrongname TO ParticipantName;
DROP TABLE Registration;
SHOW DATABASES;  SHOW TABLES;  DESC Registration;
```

## B. Git (local)

### 5. One-time setup
```
git config --global user.name "Jason Colaco"
git config --global user.email "jason.github@pm.me"
```

### 6. Branch + merge + history
```
mkdir gitpractice
cd gitpractice
nano index.html
```
Type, then Ctrl+O, Enter, Ctrl+X:
```html
<html><body><h1>Jason Colaco</h1><p>Welcome to my page</p></body></html>
```
```
git init -b main
git add index.html
git commit -m "Initial commit"
git checkout -b add-about
nano index.html          # add <h2>About</h2><p>I am a TYBCA student</p> before </body>
git add index.html
git commit -m "Add about section"
git checkout main
git merge add-about
git log --oneline --graph --all
```
(Q9: branch `add-contact`, add a Contact section.)

## C. GitHub

### 7. Login once
```
sudo apt install gh -y
gh auth login
```
GitHub.com → HTTPS → Yes → Login with a web browser → copy code → Enter → paste in browser.

### 8. CloudWorkshop + README.md
```
mkdir CloudWorkshop
cd CloudWorkshop
echo "# CloudWorkshop" > README.md
git init -b main
git add README.md
git commit -m "Add README"
gh repo create CloudWorkshop --public --source=. --push
```

### 9. Upload an existing folder
```
cd ~/gitpractice
gh repo create gitpractice --public --source=. --push
```

### 10. Q10 Pull request
```
git checkout -b update-page
nano index.html
git add .
git commit -m "Update heading"
git push -u origin update-page
gh pr create --title "Update heading" --body "Changed the main heading"
gh pr merge --merge
```

## D. Vercel — WebFiles (Goa's Wildlife)
```
mkdir WebFiles
cd WebFiles
nano index.html
```
```html
<html><head><title>Goa's Wildlife</title></head>
<body><h1>Goa's Wildlife</h1>
<p>Goa has the Bhagwan Mahavir and Cotigao wildlife sanctuaries, home to gaur, leopards and hornbills.</p>
<img src="goa.jpg" width="400">
</body></html>
```
(Put any image named goa.jpg in the folder.)
```
git init -b main
git add .
git commit -m "Goa wildlife page"
gh repo create WebFiles --public --source=. --push
```
vercel.com → Login with GitHub → Add New → Project → Import WebFiles → Framework **Other** → Deploy → open URL.

## Order to remember
- MySQL: `sudo mysql` → CREATE DB → USE → CREATE TABLE → INSERT → SELECT WHERE
- Git: init → add → commit → checkout -b → commit → checkout main → merge → log
- GitHub: `gh auth login` → `gh repo create NAME --public --source=. --push`
- Vercel: Import → Other → Deploy
