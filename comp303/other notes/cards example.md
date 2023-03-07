```java
public class Card {  

	enum Suit { 
		CLUBS(Color.BLACK), DIAMONDS(Color.RED), SPADES(Color.BLACK), HEARTS(Color.RED) 
		private Color aColor; 
		public enum Color {BLACK, RED}
		Suit(Color pColor) {
			this.aColor = pColor;
		}
		public Color getColor() {
			return this.aColor;
		}
	} 
	// Made final so that it s immutable  
	private final Suit aSuit;  
	private final Rank aRank; 
	private final Color aColor; 
	
	public Card(Rank pRank, Suit pSuit) {  
		// Add stuff to avoid null references  
		aRank = pRank;  
		aSuit = pSuit;  
		
		if (Suit = HEARTS || Suit = DIAMONDS) {
			aColor = RED
		} else {
			aColor = BLACK
		}
	}  
	public Rank getRank() {  
		return aRank;  
	}  
	// Might not want to have  
	public void setRank(Rank pRank) {  
		aRank = pRank;  
	}
	
	public class Deck { 
		private List aCards = new ArrayList<>(); 
		public List getCards() { 
		      // Create a copy so that user can’t modify original 
		      return new ArrayList<> (aCards); 
		}
}
```

