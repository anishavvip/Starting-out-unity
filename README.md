<p align="center">
  <img src="https://user-images.githubusercontent.com/85858695/151645394-6fc12fd5-def0-4bd5-9a26-7b5b36948960.png" alt="Prototypes"/>
  <img src="https://user-images.githubusercontent.com/85858695/151645105-65112d72-5876-4b37-941f-6dbf9bcfe1b6.png" alt="Cut The Rope"/>
  <img src="https://user-images.githubusercontent.com/85858695/168737228-1a879d28-7ac7-4c7d-8233-43595216a416.gif" alt="Cut The Rope"/>
</p>

# Code Snippets - Cut The Rope

**Form the rope**
- Keep connecting the mini-links to their previous link
- Except for the last one which should be connected to the candy (weight).
```C#
void Rope()
    {
        Rigidbody2D previousRB = hinge;
        for(int i = 0; i < size; i++)
        {
            GameObject link = Instantiate(linkPrefab, transform);
            HingeJoint2D joint = link.GetComponent<HingeJoint2D>();
            joint.connectedBody = previousRB;
            if (i == size - 1)
            {
                Weight(link.GetComponent<Rigidbody2D>()); 
            }
            else
            {
                previousRB = link.GetComponent<Rigidbody2D>();
            }
        }
    }
```
**Candy (weight)**
- Check the details required in the inspector for the hinge joint and add data accordingly through the script 
- Call this function while connecting the last link to the candy
```C#
void Weight(Rigidbody2D endRB)
    {
        HingeJoint2D weight = GameObject.FindGameObjectWithTag("Weight").AddComponent<HingeJoint2D>();
        weight.autoConfigureConnectedAnchor = false;
        weight.anchor = Vector2.zero;
        weight.connectedBody = endRB;
        weight.connectedAnchor = new Vector2(0, -distanceFromChainEnd);
    }
```

![br](https://user-images.githubusercontent.com/85858695/150830170-ff952e0c-23ed-4b9b-8da5-77c48049af65.png)

<p align="center">
  <img src="https://user-images.githubusercontent.com/85858695/151644561-c3d0726f-0276-4425-8b8f-6b9f93d76372.png" alt="Endless Runner"/>
  <img src="https://user-images.githubusercontent.com/85858695/168739346-c4de1831-83e2-4dba-8984-71cf92c54a70.gif" alt="Endless Runner"/>
</p>



# Code Snippets - Endless Runner

**Move in lanes**

```C#
//MOVE IN THE LANES
void MoverX(int wallNo, int direction)
    {
        for (int i = 1; i < 5; i++)
        {
            if (wallNo != i)
            {
                walls[i].isTrigger = false;
            }
            else
            {
                walls[i].isTrigger = true;
            }
        }

        move.x = direction * speed;
    }
```
**Calling function MoverX**

```C#
void Update()
    {
        move.z = Time.deltaTime * speed;

        if (Input.GetKeyDown(KeyCode.A))
        {
            if (wallNo <= 1)
            {
                wallNo = 1;
                MoverX(wallNo, -1);
            }
            else
            {
                MoverX(wallNo, -1);
                wallNo--;
            }
           
        }

        if (Input.GetKeyDown(KeyCode.D))
        {
            if (wallNo >= 4)
            {
                wallNo = 4;
                MoverX(wallNo, 1);
            }
            else
            {
                MoverX(wallNo, 1);
                wallNo++;
            }
           
        }

        Jumping(Input.GetKeyDown(KeyCode.Space));

        Sliding(Input.GetKeyDown(KeyCode.S));

        player.Move(move);
    }
```
**Making it endless**
```C#
private void OnTriggerEnter(Collider other)
    {
        if (other.tag == "Player")
        {
            Instantiate(Base, baseSpawner.position, baseSpawner.rotation);
            Destroy(GameObject.FindGameObjectWithTag("BASE"), 2f);
        }

    }
```

![br](https://user-images.githubusercontent.com/85858695/150830170-ff952e0c-23ed-4b9b-8da5-77c48049af65.png)

<p align="center">
  <img src="https://user-images.githubusercontent.com/85858695/151644524-acefbc9a-90fe-4d78-a091-95e6d82dba44.png" alt="Color Switch"/>
  <img src="https://user-images.githubusercontent.com/85858695/168735185-796ef920-ae16-4b45-8269-169ce4f2aba0.gif" alt="Color Switch"/>
</p>


# Code Snippets - Color Switch

**Collisions**

**Collision with white coin**
  - Camera reaches that location
  - The coin becomes invisible
  
**Collision with color switcher**
  - A random material is generated from the list provided
  - The material only gets assigned if it's not the same as the current material
  
**Collision with different color**
  - If name of material is different from the current material
  - If our list contains it as a registered material
   
```C#
private void OnTriggerStay2D(Collider2D collision)
    {
        string mat = gameObject.GetComponent<SpriteRenderer>().material.name;

        if (collision.tag == "WhiteCoin")
        {
            collision.gameObject.GetComponent<SpriteRenderer>().enabled = false;
            camPos = collision.gameObject.transform.position;
        }

        if (collision.tag == "ColorSwitcher")
        {
            Material newmat = material[Random.Range(0, 4)];
            if (newmat.name != mat.Split(' ')[0])
            {
                spriteRenderer.material = newmat;
            }
        }

        if (collision.gameObject.tag != mat.Split(' ')[0] && mat1.Contains(collision.gameObject.tag))
        {
            Destroy(gameObject);
        }
    }
```

![br](https://user-images.githubusercontent.com/85858695/150830170-ff952e0c-23ed-4b9b-8da5-77c48049af65.png)


<p align="center">
  <img src="https://user-images.githubusercontent.com/85858695/151644728-f2ae40ad-5be6-434a-9dc0-cdea87b483be.png" alt="Escalator"/>
  <img src="https://user-images.githubusercontent.com/85858695/168736149-2a987437-ee5b-47a6-adbc-89fec375c7cf.gif" alt="Escalator"/>
</p>

# Code Snippets - Escalator

**Pooling players**

```C#
void PlayerEnteredTheMathWall()
    {
        string str = gameObject.ToString();
        float val = float.Parse(str.Substring(0, 1));
        int poolStart = ObjectPooler.SharedInstance.lastVal;
        for (int i = poolStart; i < (int)(val + poolStart); i++)
        {
            if (!ObjectPooler.SharedInstance.pooledObjects[i].activeInHierarchy)
            {
                GameObject man = ObjectPooler.SharedInstance.pooledObjects[i];
                if (man != null)
                {
                    //Not to make it move in a straight line
                    if (i % 2 == 0)
                    {
                        man.transform.position = new Vector3(-xPos, yPos, zPos);
                        man.GetComponent<PlayerLogic>().MovePlayer();
                    }
                    else
                    {
                        man.transform.position = new Vector3(xPos, yPos, zPos);
                        man.GetComponent<PlayerLogic>().MovePlayer();
                    }
                }
                man.SetActive(true);
            }
            k++;
        }
        ObjectPooler.SharedInstance.lastVal = poolStart + (int)val;
    }
```

![br](https://user-images.githubusercontent.com/85858695/150830170-ff952e0c-23ed-4b9b-8da5-77c48049af65.png)

<p align="center">
  <img src="https://user-images.githubusercontent.com/85858695/151645173-e34e9eff-681c-4f52-9093-7216f38ab065.png" alt="aa"/>
  <img src="https://user-images.githubusercontent.com/85858695/168736257-f8601876-74d9-4f37-966e-7e5c659c1b96.gif" alt="aa"/>
</p>


# Code Snippets - aa

**Push**

```C#
void Push(int size)
    {
        for (int i = 0; i < size; i++)
        {
            GameObject ball = Instantiate(ballPrefab, ballSpawner);
            ball.transform.position += new Vector3(0, pos * i, 0);
            queue.Enqueue(ball);
            ball.GetComponent<Rigidbody2D>().constraints = RigidbodyConstraints2D.FreezePosition;
        }
    }
```
**Pop**

```C#
void Pop()
    {
        if (queue.Count != 0)
        {
            ballNew = queue.Dequeue();
            ballNew.GetComponent<Rigidbody2D>().constraints = RigidbodyConstraints2D.None;
            ballNew.GetComponent<Rigidbody2D>().velocity = Vector3.up * speed;
            ballNew.transform.parent = null;
        }
    }

void UpdatePop()
    {
        ballSpawner.position -= new Vector3(0, pos, 0);
    }
```
![br](https://user-images.githubusercontent.com/85858695/150830170-ff952e0c-23ed-4b9b-8da5-77c48049af65.png)

<p align="center">
  <img src="https://user-images.githubusercontent.com/85858695/151644906-61c8ec3c-3bd4-41d4-ad48-7088137857a2.png" alt="Basic Shooting"/>
  <img src="https://user-images.githubusercontent.com/85858695/168738784-03c62e85-402b-4816-a7e5-3bfd1fb42934.gif" alt="Basic Shooting"/>
</p>

# Code Snippets - Basic Shooting

**Switch case**

```C#
void Update()
    {
        if (angry)
        {
            currentState = EnemyStates.Chase;
        }
        switch (currentState)
        {
            case (EnemyStates.Idle):
                SearchForPlayer();
                break;

            case (EnemyStates.Attack):
                Attack();
                break;

            case (EnemyStates.Chase):
                Chase();
                break;

            case (EnemyStates.Patrol):
                SearchForPlayer();
                Patrol();
                break;
        }
    }

```
**Search for the player**

```C#
void SearchForPlayer()
    {
        float distance = Vector3.Distance(player.transform.position, transform.position);
        if (distance < aggroRadius)
        {
            currentState = EnemyStates.Chase;
        }
    }
```
**Patrol**

```C#
void Patrol()
    {
        navMeshAgent.SetDestination(currentPatrolPos);
        float distance = Vector3.Distance(currentPatrolPos, transform.position);
        if (distance < meleeRadius)
        {
            if (currentPatrolPos == PatrolStartPos.position)
            {
                currentPatrolPos = PatrolEndPos.position;
            }
            else
            {
                currentPatrolPos = PatrolStartPos.position;
            }
        }
    }
```
**Chase**

```C#
void Chase()
    {
        navMeshAgent.SetDestination(player.transform.position);
        float distance = Vector3.Distance(player.transform.position, transform.position);
        if (distance < meleeRadius)
        {
            navMeshAgent.isStopped = true;
            navMeshAgent.velocity = Vector3.zero;
            currentState = EnemyStates.Attack;
        }
        else
        {
            navMeshAgent.isStopped = false;
        }
    }
```
**Attack**

```C#
 void Attack()
    {
        float distance = Vector3.Distance(player.transform.position, transform.position);
        if (distance < meleeRadius)
        {
            currentCoolDown -= Time.deltaTime;
            if (currentCoolDown < 0)
            {
                currentState = EnemyStates.Attack;
                if (health > 0)
                {
                    health -= 0.5f;
                }
                currentCoolDown = MaxCoolDown;
            }
        }
        else
        {
            currentState = EnemyStates.Chase;
        }
    }
```
![br](https://user-images.githubusercontent.com/85858695/150830170-ff952e0c-23ed-4b9b-8da5-77c48049af65.png)

