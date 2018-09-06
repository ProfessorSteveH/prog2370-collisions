# prog2370-collisions
Simple Collision Library for Monogame (used in PROG2370)

basic usage for movement:

Player is a Rectangle, Obstacles are contained in List<Rectangle>.

in Game loop:
        1. accept input, set velocity (X and/or Y) to approrpiate value
                        (add gravity if appropriate)
        2. create a new "proposed" position player
        3. Check against all obstacles
        4. bitmask compare each direction, undoing velocity should there be an obstacle in the way.
        5. render player.

        code:

        protected override void Update(GameTime gameTime)
        {
                velocity.X = 0;             // start of every frame, we need to be motionless for constant speed

                KeyboardState keyState = Keyboard.GetState();

                if (keyState.IsKeyDown(Keys.F) || keyState.IsKeyDown(Keys.Right))
                {
                        velocity.X = SPEED;
                }

                //and the others...

                //gravity
                //https://stackoverflow.com/questions/8625167/simulating-gravity-in-xna
                velocity.Y += gravity.Y * deltaTime;

                // where we plan to go:
                Rectangle proposedPlayer = new Rectangle(player.X + (int)velocity.X,
                                                                player.Y + (int)velocity.Y,
                                                                player.Width,
                                                                player.Height);
                // will plan work?
                Sides collisionSides = proposedPlayer.CheckCollisions(floorList);

                if ((collisionSides & Sides.RIGHT) == Sides.RIGHT)
                        if (velocity.X > 0)
                                velocity.X = 0;

                    if ((collisionSides & Sides.LEFT) == Sides.LEFT)
                        if (velocity.X < 0)
                                velocity.X = 0;

                if (((collisionSides & Sides.BOTTOM) == Sides.BOTTOM) && (currentJumpPower != JUMPPOWER)) //we just started the jump, so ingnore the collision
                {
                        velocity.Y = 0;
                        isGrounded = true;
                }

~           
