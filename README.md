# Parichauhan-
Application 
import { useState } from "react";
import { Button } from "@/components/ui/button";
import { Card, CardContent } from "@/components/ui/card";
import { motion } from "framer-motion";

const ingredientsList = ["Tomato", "Cheese", "Bread", "Egg", "Chicken", "Onion", "Garlic"];

export default function CookingSimulator() {
  const [selectedIngredients, setSelectedIngredients] = useState([]);
  const [isCooking, setIsCooking] = useState(false);
  const [result, setResult] = useState("");

  const handleIngredientClick = (ingredient) => {
    if (!isCooking && !selectedIngredients.includes(ingredient)) {
      setSelectedIngredients([...selectedIngredients, ingredient]);
    }
  };

  const cook = () => {
    setIsCooking(true);
    setTimeout(() => {
      const dish = generateDish(selectedIngredients);
      setResult(dish);
      setIsCooking(false);
    }, 2000);
  };

  const generateDish = (ingredients) => {
    const combo = ingredients.sort().join("-");
    const recipes = {
      "Bread-Cheese-Tomato": "Grilled Cheese Sandwich",
      "Egg-Onion-Tomato": "Egg Bhurji",
      "Chicken-Garlic-Onion": "Garlic Chicken",
    };
    return recipes[combo] || "Interesting Mystery Dish!";
  };

  const reset = () => {
    setSelectedIngredients([]);
    setResult("");
  };

  return (
    <div className="p-6 max-w-2xl mx-auto">
      <h1 className="text-3xl font-bold mb-4 text-center">Virtual Cooking Simulator</h1>
      <div className="grid grid-cols-3 gap-4 mb-6">
        {ingredientsList.map((ingredient) => (
          <Button
            key={ingredient}
            onClick={() => handleIngredientClick(ingredient)}
            disabled={isCooking || selectedIngredients.includes(ingredient)}
            className="bg-yellow-200 hover:bg-yellow-300 text-black"
          >
            {ingredient}
          </Button>
        ))}
      </div>

      <div className="flex justify-center space-x-4 mb-4">
        <Button onClick={cook} disabled={isCooking || selectedIngredients.length === 0}>Cook</Button>
        <Button onClick={reset} variant="outline">Reset</Button>
      </div>

      {isCooking && (
        <motion.div
          className="text-center text-xl font-medium"
          animate={{ rotate: 360 }}
          transition={{ repeat: Infinity, duration: 1 }}
        >
          Cooking...
        </motion.div>
      )}

      {result && !isCooking && (
        <Card className="mt-6">
          <CardContent className="text-center text-xl font-semibold">
            🍽️ You made: {result}
          </CardContent>
        </Card>
      )}
    </div>
  );
}
