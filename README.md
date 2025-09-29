# EXP4.2-23BCS13917
Develop a REST API for playing card collection using Express.js routes.

const express = require('express');
const bodyParser = require('body-parser');

const app = express();
const PORT = 3000;

app.use(bodyParser.json());

// In-memory card collection
let cards = [
  { id: 1, suit: 'Hearts', value: 'Ace' },
  { id: 2, suit: 'Spades', value: 'King' }
];

// ✅ 1. GET all cards
app.get('/cards', (req, res) => {
  res.json(cards);
});

// ✅ 2. GET card by ID
app.get('/cards/:id', (req, res) => {
  const card = cards.find(c => c.id === parseInt(req.params.id));
  if (!card) return res.status(404).json({ message: 'Card not found' });
  res.json(card);
});

// ✅ 3. POST add new card
app.post('/cards', (req, res) => {
  const { suit, value } = req.body;
  if (!suit || !value) {
    return res.status(400).json({ message: 'Suit and value are required' });
  }
  const newCard = {
    id: cards.length ? cards[cards.length - 1].id + 1 : 1,
    suit,
    value
  };
  cards.push(newCard);
  res.status(201).json(newCard);
});

// ✅ 4. PUT update card by ID
app.put('/cards/:id', (req, res) => {
  const card = cards.find(c => c.id === parseInt(req.params.id));
  if (!card) return res.status(404).json({ message: 'Card not found' });

  const { suit, value } = req.body;
  if (suit) card.suit = suit;
  if (value) card.value = value;

  res.json(card);
});

// ✅ 5. DELETE card by ID
app.delete('/cards/:id', (req, res) => {
  const index = cards.findIndex(c => c.id === parseInt(req.params.id));
  if (index === -1) return res.status(404).json({ message: 'Card not found' });

  const deletedCard = cards.splice(index, 1);
  res.json({ message: 'Card deleted', card: deletedCard[0] });
});

// Start server
app.listen(PORT, () => {
  console.log(`🎴 Card API running at http://localhost:${PORT}`);
});
