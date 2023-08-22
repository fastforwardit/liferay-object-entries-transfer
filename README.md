# Liferay Object Entries Transfer

This web application is built using Vue.js 3 and Vite and provides a simple user interface to fetch object entries from a source Liferay URL and transfer it to a target Liferay URL using Basic Authentication.

## Features

- Allows users to input source and target URLs.
- Users can also input username and password for Basic Authentication.
- Provides status updates in real-time during the data fetching and transfer process.
- Performs data transformation between fetch and transfer. Specifically, it removes the `actions` and `id`property from each item and handles paginated results using query parameters `page` and `pageSize`.

## Installation and Setup

1. **Prerequisites:**
   - Make sure all Object Defitions are created in the target Liferay instance.
   - Make sure all Picklist Definitions are created in the target Liferay instance.
   - Ensure that you have Node.js and npm installed. If not, download and install them from [here](https://nodejs.org/).
   - Install Vue CLI globally using:
     ```bash
     npm install -g @vue/cli
     ```

2. **Clone and Install Dependencies:**

   Clone the repository to your local machine and install the required dependencies:
   ```bash
   git clone [this repo url]
   cd liferay-object-entries-transfer
   npm install
   ```

3. **Running the Application:**

   You can run the Vue app using the following command:
   ```bash
   npm run dev
   ```

   The vite app will be served at `http://localhost:5173/` (or the next available port).

## Usage

1. Open the web application in your browser.
2. Enter the source URL, target URL, and the required Basic Authentication credentials.
3. Click the "transfer data" button to start the data transfer process.
4. Watch the status messages to monitor the progress.

## Notes

- This is a simple project for demonstration purposes. Some features like error handling, security considerations, and robustness might need to be enhanced before deploying in a production environment.
- Never expose sensitive information like passwords. Always ensure you are working in a secure environment.

## Contributing

Pull requests are welcome. For major changes, please open an issue first to discuss what you would like to change.

## License

[MIT](https://choosealicense.com/licenses/mit/)
