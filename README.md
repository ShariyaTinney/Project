# Project
import java.awt.*;  
public class BrickMap {  
    public int[][] brickLayout;  
    public int brickWidth;  
    public int brickHeight;  
    
    public BrickMap(int rows, int columns) {
        brickLayout = new int[rows][columns];
        
        for (int i = 0; i < rows; i++) {
            for (int j = 0; j < columns; j++) {
                brickLayout[i][j] = 1;
            }
        }    
        
        brickWidth = 540 / columns;  
        brickHeight = 150 / rows;  
      
    }

     public void draw(Graphics2D g) {  
        for (int i = 0; i < brickLayout.length; i++) {  
            for (int j = 0; j < brickLayout[i].length; j++) {  
                if (brickLayout[i][j] > 0) {  
                    g.setColor(new Color(0xFFAA33));  
                    g.fillRect(j * brickWidth + 80, i * brickHeight + 50, brickWidth, brickHeight);  

                    g.setStroke(new BasicStroke(4));  
                    g.setColor(Color.BLACK);  
                    g.drawRect(j * brickWidth + 80, i * brickHeight + 50, brickWidth, brickHeight);  
                }  
            }  
        }  
    }  

    public void setBrickValue(int value, int row, int col) {  
        if (row >= 0 && row < brickLayout.length && col >= 0 && col < brickLayout[0].length) {
            brickLayout[row][col] = value;  
        }
    }  
}  
import java.awt.Color;
import java.awt.Font;
import java.awt.Graphics;
import java.awt.Graphics2D;
import java.awt.Rectangle;
import java.awt.event.ActionEvent;
import java.awt.event.ActionListener;
import java.awt.event.KeyEvent;
import java.awt.event.KeyListener;
import javax.swing.JPanel;
import javax.swing.Timer;
import javax.swing.SwingUtilities;

class GamePanel extends JPanel implements KeyListener, ActionListener {  
    private boolean isPlaying = true;  
    private int playerScore = 0;  
    private int totalBricks = 21;  
    private final Timer timer;  
    private final int delay = 8;  
    private int playerX = 310;  
    private int ballPosX = 120;  
    private int ballPosY = 350;  
    private int ballXDir = -1;  
    private int ballYDir = -2;  
    private BrickMap brickMap;  

    public GamePanel() {  
        brickMap = new BrickMap(3, 7);
        setFocusable(true);  
        setFocusTraversalKeysEnabled(false);  
        timer = new Timer(delay, this);  
        SwingUtilities.invokeLater(this::initListeners);  

        timer.start();  
    }  

    private void initListeners() {
        addKeyListener(this);
    }

    @Override
    public void paint(Graphics g) {  
        g.setColor(new Color(0xFFFFE0)); 
        g.fillRect(1, 1, 692, 592);  

        brickMap.draw((Graphics2D) g);  

        // Walls
        g.fillRect(0, 0, 3, 592);  

        g.fillRect(0, 0, 692, 3);  
        g.fillRect(691, 0, 3, 592);  

        // Paddle
        g.setColor(new Color(0x4169E1)); 
        g.fillRect(playerX, 550, 100, 12);  

        // Ball
        g.setColor(new Color(0xDC143C)); 
        g.fillOval(ballPosX, ballPosY, 20, 20);  

        // Score
        g.setColor(Color.black);  
        g.setFont(new Font("MV Boli", Font.BOLD, 25));  
        g.drawString("Score: " + playerScore, 520, 30);  

        // Winning Condition
        if (totalBricks <= 0) {  
            isPlaying = false;  
            ballXDir = 0;  
            ballYDir = 0;  
            g.setColor(new Color(0XFF6464));  
            g.setFont(new Font("MV Boli", Font.BOLD, 30));  
            g.drawString("You Won, Score: " + playerScore, 190, 300);  
            g.setFont(new Font("MV Boli", Font.BOLD, 20));  
            g.drawString("Press Enter to Restart.", 230, 350);  
        }  

        // Game Over Condition
        if (ballPosY > 570) {  
            isPlaying = false;  
            ballXDir = 0;  
            ballYDir = 0;  
            g.setColor(Color.BLACK);  
            g.setFont(new Font("MV Boli", Font.BOLD, 30));  
            g.drawString("Game Over, Score: " + playerScore, 190, 300);  
            g.setFont(new Font("MV Boli", Font.BOLD, 20));  
            g.drawString("Press Enter to Restart", 230, 350);  
        }  

        g.dispose();  
    }  

    @Override  
    public void actionPerformed(ActionEvent arg0) {  
        timer.start();  

        if (isPlaying) {  
            // Ball & Paddle 
            if (new Rectangle(ballPosX, ballPosY, 20, 20).intersects(new Rectangle(playerX, 550, 100, 8))) {  
                ballYDir = -ballYDir;  
            }  

            // Ball & Brick 
            for (int i = 0; i < brickMap.brickLayout.length; i++) {  
                for (int j = 0; j < brickMap.brickLayout[0].length; j++) {  
                    if (brickMap.brickLayout[i][j] > 0) {  
                        int brickX = j * brickMap.brickWidth + 80;  
                        int brickY = i * brickMap.brickHeight + 50;  
                        Rectangle brickRect = new Rectangle(brickX, brickY, brickMap.brickWidth, brickMap.brickHeight);  
                        Rectangle ballRect = new Rectangle(ballPosX, ballPosY, 20, 20);  

                        if (ballRect.intersects(brickRect)) {  
                            brickMap.setBrickValue(0, i, j);  
                            totalBricks--;  
                            playerScore += 10;  

                            // Ball Direction Change
                            if (ballPosX + 19 <= brickRect.x || ballPosX + 1 >= brickRect.x + brickRect.width)  
                                ballXDir = -ballXDir;  
                            else {  
                                ballYDir = -ballYDir;  
                            }  
                        }  
                    }  
                }  
            }  

            // Ball Movement
            ballPosX += ballXDir;  
            ballPosY += ballYDir;  

            // Ball Wall 
            if (ballPosX < 0 || ballPosX > 670) {  
                ballXDir = -ballXDir;  
            }  
            if (ballPosY < 0) {  
                ballYDir = -ballYDir;  
            }  
        }  
        repaint();  
    }  

    @Override  
    public void keyPressed(KeyEvent e) {  
        if (e.getKeyCode() == KeyEvent.VK_RIGHT) {  
            if (playerX < 600) moveRight();  
        }  
        if (e.getKeyCode() == KeyEvent.VK_LEFT) {  
            if (playerX > 10) moveLeft();  
        }  
        if (e.getKeyCode() == KeyEvent.VK_ENTER && !isPlaying) {  
            restartGame();  
        }  
    }  

    public void moveRight() {  
        isPlaying = true;  
        playerX += 50;  
    }  

    public void moveLeft() {  
        isPlaying = true;  
        playerX -= 50;  
    }  

    public void restartGame() {  
        isPlaying = true;  
        ballPosX = 120;  
        ballPosY = 350;  
        ballXDir = -1;  
        ballYDir = -2;  
        playerScore = 0;  
        totalBricks = 21;  
        brickMap = new BrickMap(3, 7);  
        repaint();  
    }  

    @Override public void keyReleased(KeyEvent e) {}  
    @Override public void keyTyped(KeyEvent e) {}  
}
import javax.swing.JFrame;

public class BrickBreakerGame {
    public static void main(String[] args) {
        JFrame frame = new JFrame("Brick Breaker");
        GamePanel gamePanel = new GamePanel();

        frame.setBounds(10, 10, 700, 600);
        frame.setResizable(false);
        frame.setDefaultCloseOperation(JFrame.EXIT_ON_CLOSE);
        frame.add(gamePanel);
        frame.setVisible(true);
    }
}


