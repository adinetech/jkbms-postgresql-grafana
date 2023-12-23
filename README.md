## Installation

1. Install the `mppsolar` Python package with BLE and PostgreSQL support:

    ```bash
    pip install mppsolar[ble,pgsql]
    ```

    If you encounter errors related to `bluepy`, install it and its dependencies:

    ```bash
    sudo pip3 install bluepy
    ```

    If `bluepy` installation still fails, try installing the required development library:

    ```bash
    sudo apt-get install libglib2.0-dev
    ```

    Afterward, run the first `pip install mppsolar[ble,pgsql]` command again.

2. Install the PostgreSQL development library and `psycopg2`:

    ```bash
    sudo apt-get install libpq-dev
    sudo pip3 install psycopg2
    ```
    Example: `export PATH=$PATH:/home/pi/.local/bin`

3. Add the `mppsolar` directory to your PATH for convenient access by running the following command:

    ```bash
    export PATH=$PATH:/path/to/mppsolar
    ```

    Alternatively, to make the change persistent across sessions, add the above export command to your shell profile configuration file (e.g., `.bashrc` or `.zshrc`).

    ```bash
    echo 'export PATH=$PATH:/home/pi/.local/bin' >> ~/.bashrc   # For Bash
    # or
    echo 'export PATH=$PATH:/home/pi/.local/bin' >> ~/.zshrc    # For Zsh
    ```

    Then, run `source ~/.bashrc` (or `source ~/.zshrc`) or reopen your terminal.

## Database Setup

1. Install PostgreSQL and its contrib package:

    ```bash
    sudo apt-get install postgresql postgresql-contrib
    ```

2. Start and enable the PostgreSQL service:

    ```bash
    sudo systemctl start postgresql
    sudo systemctl enable postgresql
    ```

3. Create a PostgreSQL user:

    ```bash
    sudo -u postgres createuser --interactive
    ```

4. Create a PostgreSQL database:

    ```bash
    sudo -u postgres createdb your_database_name
    ```

    For example:

    ```bash
    sudo -u postgres createdb battery
    ```

5. Access the PostgreSQL shell:

    ```bash
    sudo -u postgres psql
    ```

6. Set/change the password for the newly created user:

    ```sql
    ALTER USER your_username WITH PASSWORD 'your_password';
    ```

7. Edit the `pg_hba.conf` file:

    ```bash
    sudo nano /etc/postgresql/{version}/main/pg_hba.conf
    ```

    Change the authentication method of your_username from `peer` to `md5`.

8. Restart the PostgreSQL service:

    ```bash
    sudo systemctl restart postgresql
    ```
## Get JKBMS's MAC Address

1. Run `blescan` with sudo to discover nearby BLE devices:

    ```bash
    sudo blescan
    ```
    Note: The MAC address of your JKBMS device is the alphanumeric string following "Device (new):

## Usage

- Run the mppsolar tool to collect data and store it in the PostgreSQL database:

    ```bash
    mppsolar -p your_device_mac -P JK02 -c getCellData -o postgres --postgres_url postgresql://your_username:your_password@localhost:5432/your_database_name
    ```

    Replace placeholders (`your_device_mac`, `your_username`, `your_password`, `your_database_name`) with your actual values.

    Note: The `-o postgres` option specifies that the tool will export data to the provided PostgreSQL database. It won't return any output.

## Automate Data Collection with Cron Job

- Set up a cron job to run the mppsolar tool at specified intervals. For example, to run every minute:

    ```bash
    * * * * * /home/pi/.local/bin/mppsolar -p your_device_mac -P JK02 -c getCellData -o postgres --postgres_url postgresql://your_username:your_password@localhost:5432/your_database_name
    ```

    Adjust the cron job command with your actual path to the mppsolar tool, device MAC address, and PostgreSQL database URL.

## Contributing

Feel free to contribute to this project by submitting issues or pull requests.

## License

This project is licensed under the [MIT License](LICENSE).
