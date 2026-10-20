Modern React + Node webapp for user-seeded movie recommendations

Quick start (requires Node.js >= 16)

1. Server
   cd server
   npm install
   npm start

2. Client
   cd client
   npm install
   npm run dev

The server expects the MovieLens small dataset at `data/raw/ml-latest-small/movies.csv` (same as used elsewhere in the project).

The client provides a 5-input UI and calls the server's `/recommend` endpoint. Use the placeholder posters or wire up real poster URLs later.
