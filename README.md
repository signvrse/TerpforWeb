**The Installation Process of TerpWeb Documentation.**

## 1. Install the prerequisites

Open PowerShell and install Git, Node.js, Java 25, and Docker Desktop:

```powershell
winget install --id Git.Git --exact
winget install --id OpenJS.NodeJS.LTS --exact
winget install --id EclipseAdoptium.Temurin.25.JDK --exact
winget install --id Docker.DockerDesktop --exact
```

If an installer asks you to restart, do so. Then launch **Docker Desktop** and wait for it to finish starting. Verify the tools are available in a new PowerShell window:

```powershell
git --version
node --version
npm --version
java -version
docker compose version
```

The backend requires Java 25. Docker Desktop supplies PostgreSQL for the local app.

## 2. Clone the repository

These commands clone the project into a `src` folder under your Windows user profile:

```powershell
New-Item -ItemType Directory -Force "$HOME\src" | Out-Null
Set-Location "$HOME\src"
git clone https://github.com/signvrse/terp-for-web.git
Set-Location "$HOME\src\terp-for-web"
```

## 3. Install dashboard dependencies and start the database

From the repository directory:

```powershell
Set-Location "$HOME\src\terp-for-web\dashboard"
npm ci
```

Then start the PostgreSQL container from the repository’s `docker-compose.yml`. Leave Docker Desktop running:

```powershell
Set-Location "$HOME\src\terp-for-web"
docker compose up -d db
docker compose ps
```

The database is configured for local development on port `5433`, with username and password `postgres`.

## 4. Start the backend API

Open a **second PowerShell window** and paste this block. It creates temporary signing keys for local development and starts the API. The first run may take several minutes while Maven downloads dependencies.

```powershell
Set-Location "$HOME\src\terp-for-web\backend"

$jdk = "$env:LOCALAPPDATA\Programs\Eclipse Adoptium\jdk-25.0.4.1+1"
$env:JAVA_HOME = $jdk
$env:Path = "$jdk\bin;$env:Path"

$env:DB_USER = 'postgres'
$env:DB_PASS = 'postgres'
$env:SERVER_ADDRESS = '127.0.0.1'
$env:SUPER_ADMIN_EMAIL = 'admin@localhost.test'
$env:SUPER_ADMIN_PASSWORD = 'TerpLocalDev!25'

$keyFile = Join-Path $env:TEMP 'GenerateTerpJwtKeys.java'
$source = @'
import java.security.KeyPairGenerator;
import java.util.Base64;

public class GenerateTerpJwtKeys {
    public static void main(String[] args) throws Exception {
        var generator = KeyPairGenerator.getInstance("RSA");
        generator.initialize(2048);
        var pair = generator.generateKeyPair();
        System.out.println("PRIVATE=" + Base64.getEncoder().encodeToString(pair.getPrivate().getEncoded()));
        System.out.println("PUBLIC=" + Base64.getEncoder().encodeToString(pair.getPublic().getEncoded()));
    }
}
'@

try {
    [System.IO.File]::WriteAllText($keyFile, $source, [System.Text.Encoding]::ASCII)
    $keys = @(& "$jdk\bin\java.exe" $keyFile)
    if ($LASTEXITCODE -ne 0) { throw 'Could not generate local JWT keys.' }

    $env:JWT_PRIVATE_KEY = ($keys | Where-Object { $_ -like 'PRIVATE=*' }).Substring(8)
    $env:JWT_PUBLIC_KEY = ($keys | Where-Object { $_ -like 'PUBLIC=*' }).Substring(7)
} finally {
    Remove-Item -LiteralPath $keyFile -Force -ErrorAction SilentlyContinue
}

.\mvnw.cmd spring-boot:run
```

Keep this window open while using the app. The API applies database migrations and creates development data when it starts.

## 5. Start the dashboard

Open a **third PowerShell window**:

```powershell
Set-Location "$HOME\src\terp-for-web\dashboard"
$env:NEXT_PUBLIC_API_URL = 'http://127.0.0.1:8080'
& '.\node_modules\.bin\next.cmd' dev -H 127.0.0.1
```

Keep this window open too. Open **[http://127.0.0.1:3000](http://127.0.0.1:3000)** in your browser and log in:

- **Email:** `admin@localhost.test`
- **Password:** `TerpLocalDev!25`

## Next time you run the project

You don’t need to clone or reinstall it again. Start Docker Desktop, then run `docker compose up -d db` from the repository folder. Start the backend and dashboard again using steps 4 and 5.

The API signing keys are generated each time you start the backend, so you’ll need to sign in again after an API restart. Email, Google sign-in, and payments may require real service credentials; they aren’t needed to start the local app.






Everything is installed and running on this PC now. You can open the dashboard at **[http://127.0.0.1:3000](http://127.0.0.1:3000)** and sign in at **[http://127.0.0.1:3000/login](http://127.0.0.1:3000/login)** using:

- **Email:** `admin@localhost.test`
- **Password:** `TerpLocalDev!25`

To start it yourself in the future, use three PowerShell windows and leave the API and dashboard windows open while you work.

### 1. Start and check PostgreSQL

PostgreSQL runs as a Windows service; the app database is `terp_db` on port `5433`.

```powershell
Get-Service postgresql-x64-17
```

If the service is stopped, start it from an elevated PowerShell window:

```powershell
Start-Service postgresql-x64-17
```

Check that the app database is available:

```powershell
$env:PGPASSWORD = 'postgres'
& 'C:\Program Files\PostgreSQL\18\bin\psql.exe' -h 127.0.0.1 -p 5433 -U postgres -d terp_db -c 'select 1;'
Remove-Item Env:PGPASSWORD
```

### 2. Start the backend API

In the second PowerShell window, paste this block. It generates temporary signing keys for local login, then starts the API; the first run may take a while as Maven downloads its dependencies.

```powershell
Set-Location 'C:\Users\USER\Desktop\My Sites\terp-for-web\backend'

$jdk = "$env:LOCALAPPDATA\Programs\Eclipse Adoptium\jdk-25.0.4.1+1"
$env:JAVA_HOME = $jdk
$env:Path = "$jdk\bin;$env:Path"

$env:DB_USER = 'postgres'
$env:DB_PASS = 'postgres'
$env:SERVER_ADDRESS = '127.0.0.1'
$env:SUPER_ADMIN_EMAIL = 'admin@localhost.test'
$env:SUPER_ADMIN_PASSWORD = 'TerpLocalDev!25'

$keyFile = Join-Path $env:TEMP 'GenerateTerpJwtKeys.java'
$source = @'
import java.security.KeyPairGenerator;
import java.util.Base64;

public class GenerateTerpJwtKeys {
    public static void main(String[] args) throws Exception {
        var generator = KeyPairGenerator.getInstance("RSA");
        generator.initialize(2048);
        var pair = generator.generateKeyPair();
        System.out.println("PRIVATE=" + Base64.getEncoder().encodeToString(pair.getPrivate().getEncoded()));
        System.out.println("PUBLIC=" + Base64.getEncoder().encodeToString(pair.getPublic().getEncoded()));
    }
}
'@

try {
    [System.IO.File]::WriteAllText($keyFile, $source, [System.Text.Encoding]::ASCII)
    $keys = @(& "$jdk\bin\java.exe" $keyFile)
    if ($LASTEXITCODE -ne 0) { throw 'Could not generate local JWT keys.' }

    $env:JWT_PRIVATE_KEY = ($keys | Where-Object { $_ -like 'PRIVATE=*' }).Substring(8)
    $env:JWT_PUBLIC_KEY = ($keys | Where-Object { $_ -like 'PUBLIC=*' }).Substring(7)
} finally {
    Remove-Item -LiteralPath $keyFile -Force -ErrorAction SilentlyContinue
}

.\mvnw.cmd spring-boot:run
```

The API runs at **[http://127.0.0.1:8080](http://127.0.0.1:8080)**. Database migrations and development seed data run automatically.

### 3. Start the dashboard

In the third PowerShell window:

```powershell
Set-Location 'C:\Users\USER\Desktop\My Sites\terp-for-web\dashboard'

$env:NEXT_PUBLIC_API_URL = 'http://127.0.0.1:8080'
& '.\node_modules\.bin\next.cmd' dev -H 127.0.0.1
```

The dashboard runs at **[http://127.0.0.1:3000](http://127.0.0.1:3000)**. If you ever need to reinstall its packages, run `npm ci` from the `dashboard` directory.

**A couple of notes:** the JWT keys are generated for each API launch, so restarting the API invalidates existing sessions; just sign in again. Email delivery, Google sign-in, and payment features may need real service credentials to work. PostgreSQL is currently listening on all network interfaces; I’ve configured it to use localhost on its next restart. Until it’s restarted, don’t use this setup on an untrusted network.
