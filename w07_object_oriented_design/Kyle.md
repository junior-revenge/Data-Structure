
... What is BlackJack?
... Java or Python?

```python
from __future__ import annotations

import random
from dataclasses import dataclass
from enum import Enum, auto
from typing import Generic, Iterable, List, Sequence, TypeVar

# ────────────────────────── Enums ──────────────────────────── #
class Suit(Enum):
    CLUBS    = auto()
    DIAMONDS = auto()
    HEARTS   = auto()
    SPADES   = auto()

    def symbol(self) -> str:
        return {"CLUBS": "♣", "DIAMONDS": "♦", "HEARTS": "♥", "SPADES": "♠"}[self.name]


class Rank(Enum):
    ACE   = 1
    TWO   = 2
    THREE = 3
    FOUR  = 4
    FIVE  = 5
    SIX   = 6
    SEVEN = 7
    EIGHT = 8
    NINE  = 9
    TEN   = 10
    JACK  = 11
    QUEEN = 12
    KING  = 13

    @property
    def short(self) -> str:
        return {
            Rank.ACE: "A",
            Rank.JACK: "J",
            Rank.QUEEN: "Q",
            Rank.KING: "K",
        }.get(self, str(self.value))


# ────────────────────────── Card ───────────────────────────── #
@dataclass(frozen=True, slots=True)
class Card:
    suit: Suit
    rank: Rank

    def __str__(self) -> str:
        return f"{self.rank.short}{self.suit.symbol()}"

    # Generic games can override this if cards have special values
    def numeric_value(self) -> int:
        return self.rank.value


# ────────────────────────── Deck ───────────────────────────── #
T = TypeVar("T", bound=Card)

class Deck(Generic[T]):
    """A stack of shuffled cards. Generic so it can host any Card subclass."""

    def __init__(self, cards: Iterable[T] | None = None) -> None:
        self._cards: List[T] = list(cards) if cards is not None else self._full_standard_deck()
        self.shuffle()

    # ----- Public API ---------------------------------------------------- #
    def shuffle(self) -> None:
        random.shuffle(self._cards)

    def remaining(self) -> int:
        return len(self._cards)

    def deal_one(self) -> T:
        if not self._cards:
            raise IndexError("No cards left in the deck.")
        return self._cards.pop()

    def deal(self, n: int) -> List[T]:
        if n < 0:
            raise ValueError("n must be non-negative.")
        if n > self.remaining():
            raise IndexError("Not enough cards left to deal.")
        return [self.deal_one() for _ in range(n)]

    # ----- Internals ----------------------------------------------------- #
    @staticmethod
    def _full_standard_deck() -> List[Card]:
        return [Card(suit, rank) for suit in Suit for rank in Rank]

# ────────────────────────── Hand ───────────────────────────── #
class Hand(Generic[T]):
    """A player’s personal pile of cards."""

    def __init__(self, cards: Iterable[T] | None = None):
        self._cards: List[T] = list(cards) if cards else []

    def add(self, card: T) -> None:
        self._cards.append(card)

    def extend(self, cards: Sequence[T]) -> None:
        self._cards.extend(cards)

    def discard_all(self) -> None:
        self._cards.clear()

    def cards(self) -> List[T]:
        return self._cards.copy()

    def __str__(self) -> str:
        return " ".join(map(str, self._cards)) or "∅ (empty hand)"


# ────────────────── Blackjack-specific Card ────────────────── #
class BlackjackCard(Card):
    """A playing card with Blackjack point values."""

    def is_ace(self) -> bool:
        return self.rank is Rank.ACE

    # --- override ------------------------------------------------------ #
    def numeric_value(self) -> int:
        """Return the *hard* value (Ace = 1)."""
        if self.rank in (Rank.JACK, Rank.QUEEN, Rank.KING):
            return 10
        return 1 if self.is_ace() else self.rank.value

    # keep Card.__str__ → shows “A♠”, “10♥”, etc.


# ────────────────── Blackjack-specific Hand ────────────────── #
class BlackjackHand(Hand[BlackjackCard]):
    """Adds scoring logic and convenience helpers."""

    def score(self) -> int:
        """
        Compute the best score: highest value ≤ 21,
        or the minimal bust score if all totals bust.
        """
        base = 0
        aces = 0
        for card in self._cards:
            base += card.numeric_value()   # Ace counts as 1 for now
            if card.is_ace():
                aces += 1

        # Upgrade some aces from 1 → 11 (adds +10) as long as we stay ≤ 21
        best = base
        while aces and best + 10 <= 21:
            best += 10
            aces -= 1
        return best

    def is_blackjack(self) -> bool:
        """Exactly two cards totaling 21."""
        return len(self._cards) == 2 and self.score() == 21

    def is_busted(self) -> bool:
        return self.score() > 21

```