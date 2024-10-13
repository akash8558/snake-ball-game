import React, { useState, useEffect, useCallback } from 'react'
import { Button } from "@/components/ui/button"

const GRID_SIZE = 20
const CELL_SIZE = 24
const INITIAL_SNAKE = [{ x: 11, y: 10 }, { x: 10, y: 10 }, { x: 9, y: 10 }]
const INITIAL_DIRECTION = { x: 1, y: 0 }
const INITIAL_APPLE = { x: 15, y: 15 }
const GAME_SPEED = 110

type Position = { x: number; y: number }
type Direction = { x: number; y: number }

enum CellType {
  Empty,
  Snake,
  Apple,
}

const SPRITES = {
  snake: "https://hebbkx1anhila5yf.public.blob.vercel-storage.com/image-59dna3xBLMzhpRL0QuOPVIqaXBHN4X.png",
  apple: "https://hebbkx1anhila5yf.public.blob.vercel-storage.com/ball-EdFwzuMXwtgMrSOXRNiPxHXKHVvvzV.png"
}

const SnakeGame: React.FC = () => {
  const [snake, setSnake] = useState<Position[]>(INITIAL_SNAKE)
  const [direction, setDirection] = useState<Direction>(INITIAL_DIRECTION)
  const [nextDirection, setNextDirection] = useState<Direction>(INITIAL_DIRECTION)
  const [apple, setApple] = useState<Position>(INITIAL_APPLE)
  const [gameOver, setGameOver] = useState(false)

  const moveSnake = useCallback(() => {
    if (gameOver) return

    setDirection(nextDirection)

    const newSnake = [...snake]
    const head = { ...newSnake[0] }
    head.x += nextDirection.x
    head.y += nextDirection.y

    if (
      head.x < 0 ||
      head.x >= GRID_SIZE ||
      head.y < 0 ||
      head.y >= GRID_SIZE ||
      newSnake.some((segment) => segment.x === head.x && segment.y === head.y)
    ) {
      setGameOver(true)
      return
    }

    newSnake.unshift(head)

    if (head.x === apple.x && head.y === apple.y) {
      setApple(getRandomPosition())
    } else {
      newSnake.pop()
    }

    setSnake(newSnake)
  }, [snake, nextDirection, apple, gameOver])

  useEffect(() => {
    const handleKeyPress = (e: KeyboardEvent) => {
      switch (e.key) {
        case 'ArrowUp':
          setNextDirection((prev) => prev.y === 0 ? { x: 0, y: -1 } : prev)
          break
        case 'ArrowDown':
          setNextDirection((prev) => prev.y === 0 ? { x: 0, y: 1 } : prev)
          break
        case 'ArrowLeft':
          setNextDirection((prev) => prev.x === 0 ? { x: -1, y: 0 } : prev)
          break
        case 'ArrowRight':
          setNextDirection((prev) => prev.x === 0 ? { x: 1, y: 0 } : prev)
          break
      }
    }

    window.addEventListener('keydown', handleKeyPress)

    return () => {
      window.removeEventListener('keydown', handleKeyPress)
    }
  }, [])

  useEffect(() => {
    const gameLoop = setInterval(moveSnake, GAME_SPEED)
    return () => clearInterval(gameLoop)
  }, [moveSnake])

  const getRandomPosition = (): Position => {
    return {
      x: Math.floor(Math.random() * GRID_SIZE),
      y: Math.floor(Math.random() * GRID_SIZE),
    }
  }

  const resetGame = () => {
    setSnake(INITIAL_SNAKE)
    setDirection(INITIAL_DIRECTION)
    setNextDirection(INITIAL_DIRECTION)
    setApple(getRandomPosition())
    setGameOver(false)
  }

  const getCellType = (x: number, y: number): CellType => {
    if (snake.some((segment) => segment.x === x && segment.y === y)) return CellType.Snake
    if (apple.x === x && apple.y === y) return CellType.Apple
    return CellType.Empty
  }

  return (
    <div className="flex flex-col items-center justify-center bg-gray-100 p-4 rounded-lg shadow-md">
      <div
        className="grid bg-white border-2 border-gray-300"
        style={{
          gridTemplateColumns: `repeat(${GRID_SIZE}, ${CELL_SIZE}px)`,
          gridTemplateRows: `repeat(${GRID_SIZE}, ${CELL_SIZE}px)`,
        }}
      >
        {Array.from({ length: GRID_SIZE * GRID_SIZE }).map((_, index) => {
          const x = index % GRID_SIZE
          const y = Math.floor(index / GRID_SIZE)
          const cellType = getCellType(x, y)

          return (
            <div
              key={index}
              style={{
                width: CELL_SIZE,
                height: CELL_SIZE,
                padding: 0,
                boxSizing: 'border-box',
              }}
            >
              {cellType === CellType.Snake && (
                <img
                  src={SPRITES.snake}
                  alt="Snake"
                  className="w-full h-full object-cover"
                />
              )}
              {cellType === CellType.Apple && (
                <img src={SPRITES.apple} alt="Apple" className="w-full h-full object-cover" />
              )}
            </div>
          )
        })}
      </div>
      {gameOver && (
        <div className="mt-4 text-center">
          <p className="text-xl font-bold mb-2">Game Over!</p>
          <Button onClick={resetGame} className="bg-blue-500 text-white px-4 py-2 rounded hover:bg-blue-600">Restart</Button>
        </div>
      )}
      <div className="mt-4 text-center">
        <p className="text-sm text-gray-600">Use arrow keys to control the snake</p>
      </div>
    </div>
  )
}

export default SnakeGame
