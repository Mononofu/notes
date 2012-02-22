a sudoku solver, fonud on julia mailing list.

first version posted

```julia
function f()
        tic()
        matrix = zeros(Int,9,9)
        j = 1
        i = 1
        for c = "3...8.......7....51..............36...2..4....7...........6.13..452...........8.."
        #for c = readline(open("../performance/1"))
                if (c != '.' && c != 10)
                        matrix[i,j] = convert(Int,c-48);
                end
                i+=1
                if (i==10)
                        i = 1
                        j += 1
                end
        end
        toc();tic();
        if (solve(1, 1, matrix))
                println(matrix)
        else
                println(":(")
        end
        toc()
end
 
function solve(i, j, matrix::Array{Int,2})
        if (i == 10)
        i = 0;
        if ((j+=1) == 10)
            return true
        end
    end
    if (matrix[i,j] != 0)
        return solve(i+1, j, matrix)
        end
        for val = 1:9
                if (legal(i, j, val, matrix))
                        matrix[i,j] = val
                        if (solve(i, j, matrix))
                                return true
                        end
                end
        end
        matrix[i,j] = 0
    return false
end
 
function legal(i, j, val, matrix::Array{Int,2})
        for k = 1:9
                if (matrix[k,j] == val)
                        return false;
                end
        end
        for k = 1:9
                if (matrix[i,k] == val)
                        return false;
                end
        end
        boxRowOffset = int(floor((i-1) / 3) * 3)
        boxColOffset = int(floor((j-1) / 3) * 3)
        for k = 1:3
                for m = 1:3
                        if (matrix[boxRowOffset+k,boxColOffset+m] == val)
                                return false
                        end
                end
        end
        return true
end
 
f()
```

second version posted, more idiosyncratic

```julia
function solve(i, j, matrix::Array{Int,2})
    if i == 10
        i = 0
        if (j+=1) == 10
            return true
        end
    end
    if matrix[i,j] != 0
        return solve(i+1, j, matrix)
    end
    for val = 1:9
        if legal(i, j, val, matrix)
            matrix[i,j] = val
            if solve(i, j, matrix)
                return true
            end
        end
    end
    matrix[i,j] = 0
    return false
end

function legal(i, j, val, matrix::Array{Int,2})
    for k = 1:9
        if matrix[k,j] == val
            return false
        end
    end
    for k = 1:9
        if matrix[i,k] == val
            return false
        end
    end
    boxRowOffset = 3*div(i-1,3)
    boxColOffset = 3*div(j-1,3)
    for k = 1:3
        for m = 1:3
            if matrix[boxRowOffset+k,boxColOffset+m] == val
                return false
            end
        end
    end
    return true
end

puzzle = "3...8.......7....51..............36...2..4....7...........6.13..452...........8.."
matrix = map(c -> c=='.' ? 0 : c-'0', reshape(puzzle.data, (9,9)))

@time begin
    if solve(1, 1, matrix)
        println(matrix)
    else
        println(":(")
    end
end
```