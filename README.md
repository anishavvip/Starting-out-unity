# Starting-out-unity

## Frogger 
https://user-images.githubusercontent.com/85858695/148811516-f6eed2f8-1ee5-4a16-a3f5-4a71efba7ef9.mp4






### Code Snippets

#### Move down in arcade style

```C#
transform.Translate(0, -intensity * speed, 0);
```

# 
## Cut the rope
https://user-images.githubusercontent.com/85858695/148811530-550be6e6-3d0a-46e2-aa89-07048c8b820c.mp4



### Code Snippets


#### Form the rope 
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
#### Candy (weight) 
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
# 
## Color switch
https://user-images.githubusercontent.com/85858695/148812220-7472ba4f-8f86-49cf-a9ac-42aba03004c6.mp4




### Code Snippets

#### Collisions
##### Collision with white coin
  - Camera reaches that location
  - The coin becomes invisible
  
##### Collision with color switcher
  - A random material is generated from the list provided
  - The material only gets assigned if it's not the same as the current material
  
##### Collision with different color
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
# 
## AA
https://user-images.githubusercontent.com/85858695/148813946-16b4a867-d58a-4c1f-99d6-59d391d68ec1.mp4



### Code Snippets

#### Push

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
#### Pop

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
# 
## Basic shooting
https://user-images.githubusercontent.com/85858695/148815664-250a4673-4b09-4bb5-8280-3095160255f8.mp4



### Code Snippets

#### Switch case

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
#### Search for the player

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
#### Patrol

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
#### Chase

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
#### Attack

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
# 
## Escalator
https://user-images.githubusercontent.com/85858695/148816077-438462aa-91c3-4f7d-818c-3576d252bfc1.mp4



### Code Snippets

#### Move down in arcade style

```C#
transform.Translate(0, -intensity * speed, 0);
```

# 
## Endless runner
https://user-images.githubusercontent.com/85858695/148812892-f007dc7f-b69d-45eb-9f67-6d358986f182.mp4




### Code Snippets

#### Move down in arcade style

```C#
transform.Translate(0, -intensity * speed, 0);
```

# 
