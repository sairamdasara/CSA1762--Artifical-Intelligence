hanoi(0, _, _, _) :- !.  % Base case: No move needed for 0 disks.
hanoi(N, Source, Destination, Auxiliary) :-
    N > 0,
    M is N - 1,
    hanoi(M, Source, Auxiliary, Destination),
    format('Move disk ~w from ~w to ~w~n', [N, Source, Destination]),
    hanoi(M, Auxiliary, Destination, Source).
