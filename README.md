from sys import stdin


class MatrixError(BaseException):
    def __init__(self, matrix1, matrix2):
        self.matrix1 = matrix1
        self.matrix2 = matrix2


class Matrix:
    def __init__(self, m):
        self.m = [row[:] for row in m]

    def __str__(self):
        return '\n'.join(map(lambda x: '\t'.join(map(str, x)), self.m))

    def size(self):
        k = list()
        k.append(len(self.m))
        k.append(len(self.m[0]))
        t = tuple(k)
        return t

    def __add__(self, other):
        if self.size() == other.size():
            o = other.m
            ans = []
            for row in range(len(self.m)):
                s = list(map(lambda x, y: x + y, self.m[row], o[row]))
                ans.append(s)
        else:
            raise MatrixError(self, other)
        return Matrix(ans)

    def __mul__(self, other):
        ans = list()
        if isinstance(other, Matrix) and \
                (self.size()[1] == other.size()[0]):
            oth = Matrix.transposed(other)
            for row in range(len(self.m)):
                st = []
                for row1 in range(len(oth.m)):
                    st.append(sum((list(map(lambda x: x[0] * x[1],
                                            list(zip(self.m[row],
                                                     oth.m[row1])))))))
                ans.append(st)

        elif isinstance(other, Matrix) and \
                self.size()[1] != other.size()[0]:
            raise MatrixError(self, other)
        else:
            for row in range(len(self.m)):
                s = list(map(lambda x: x * other, self.m[row]))
                ans.append(s)
        return Matrix(ans)

    __rmul__ = __mul__

    def transpose(self):
        nw = []
        for col in range(len(self.m[0])):
            st = []
            for row in range(len(self.m)):
                st.append(self.m[row][col])
            nw.append(st)
        self.m = nw
        return self

    @staticmethod
    def transposed(dr):
        cop = dr.m
        nw = []
        for col in range(len(cop[0])):
            st = []
            for row in range(len(cop)):
                st.append(cop[row][col])
            nw.append(st)
        cop = nw
        return Matrix(cop)


exec(stdin.read())

